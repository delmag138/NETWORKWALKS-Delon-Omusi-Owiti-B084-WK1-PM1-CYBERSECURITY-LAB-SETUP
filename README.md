# 🐍⚙️ Kali Linux Lab Environment Setup

Building an isolated virtual lab in VirtualBox with Kali Linux for practising penetration testing, network analysis, and vulnerability assessment safely.

---

## 1. Goal 🥅
To build a safe, isolated, pre-configured environment for penetration testing, ethical hacking, digital forensics, and vulnerability assessment.

1. **Safety and isolation:** I can run scripts and tools in a contained place that cannot harm my main system.
2. **Pre-loaded security toolkit:** Kali comes with hundreds of security tools ready to use.
3. **Snapshots and easy recovery:** I can restore the VM to a known-good state if something breaks.
4. **Portability and low resource needs:** the lab takes little space and runs on a normal laptop.

## 2. My Lab at a Glance

| Item | My setting |
|------|------------|
| Host OS | Windows 11 26H2 |
| VirtualBox version | VirtualBox 7.2.20 |
| Guest OS | Kali Linux 2026.2 |
| Network type and name | NAT Network (`NatNetwork`) |
| Network range | 10.0.0.0/24 (usable addresses 10.0.0.1 to 10.0.0.254) |
| Kali IP / gateway / DNS | 10.0.0.2 / 10.0.0.1 / 8.8.8.8 |

## 3. How I Built It

### Step 1: VirtualBox installation
I downloaded VirtualBox from its official website and followed the installer. I then created the Kali virtual machine and adjusted the display, CPU, and memory settings to make it stable.

<img width="960" height="822" alt="VirtualBox with the Kali VM" src="https://github.com/user-attachments/assets/83478bc0-57bc-4fe2-aac1-9708257562ae" />

<img width="1919" height="1031" alt="VM display, CPU and memory settings" src="https://github.com/user-attachments/assets/73309d0f-4ec4-474b-aa2f-4ac3045c33a9" />

After this, Kali booted and I reached the sign-in screen.

<img width="960" height="1032" alt="Kali sign-in screen" src="https://github.com/user-attachments/assets/30b430c1-903a-43b8-95a3-5d5274a97363" />

### Step 2: Creating the NAT Network
In VirtualBox's Network Manager, I created a NAT Network called `NatNetwork` with the range `10.0.0.0/24` and DHCP enabled. This is the virtual network the lab machines will share. No VM is connected to it yet at this stage.

<img width="961" height="1028" alt="NatNetwork settings in Network Manager" src="https://github.com/user-attachments/assets/69d2031d-121b-493f-b9d9-5f08adc4722a" />

### Step 3: Installing Kali and attaching the adapter
I downloaded the Kali ISO from kali.org and used it to create the VM, choosing Debian (64-bit) as the OS type.

<img width="788" height="552" alt="Creating the Kali VM" src="https://github.com/user-attachments/assets/2c0ba848-f621-45bf-a5ca-f2bc03d84d5c" />

Then I opened Settings > Network > Adapter 1 and changed **Attached to** from NAT to **NAT Network**, selecting `NatNetwork`. (The adapter was on plain NAT at first, which caused my connection problem described in section 5.)

<img width="781" height="518" alt="Adapter 1 attached to NatNetwork" src="https://github.com/user-attachments/assets/6661cf6a-b9e3-4dd6-a26c-68a82ac291b0" />

### Step 4: Setting the IP address
I set a static IPv4 address of 10.0.0.2/24 on Kali, with 10.0.0.1 as the default gateway and 8.8.8.8 as the DNS server.

<img width="698" height="547" alt="Static IP settings in Kali" src="https://github.com/user-attachments/assets/ac02adf0-aea0-48f2-ad27-16ac34d11699" />

### Step 5: Shared clipboard and snapshot
Copy and paste between Windows and Kali did not work at first. The VM's **Shared Clipboard** setting was Disabled, so I changed it to **Bidirectional**, and copying in both directions worked.

<img width="781" height="514" alt="Shared Clipboard set to Bidirectional" src="https://github.com/user-attachments/assets/e41655c4-d737-4f92-8254-8c11ccb23566" />

Finally, I took a snapshot of the working VM so I can restore it to a safe state.

<img width="955" height="365" alt="Snapshot of the Kali VM" src="https://github.com/user-attachments/assets/f979d608-e563-4817-870e-5fbff62485e2" />

## 4. Proof It Works 🧾

| Test | Command | What I saw |
|------|---------|------------|
| My IP | `ip a` | `eth0` has 10.0.0.2/24 |
| Gateway | `ping -c 4 10.0.0.1` | 4 sent, 4 received, 0% packet loss |
| Internet | `ping -c 4 8.8.8.8` | 4 sent, 4 received, 0% packet loss |
| DNS | `nslookup networkwalks.com` | The domain name resolved to an IP address |

**My IP:**

<img width="644" height="515" alt="ip a output" src="https://github.com/user-attachments/assets/98efd0d9-20c4-4bd9-b6af-78dff12ad124" />

**Gateway:**

<img width="651" height="521" alt="Gateway ping" src="https://github.com/user-attachments/assets/f4d439a3-80e0-4af1-9eef-776626e875c0" />

**Internet:**

<img width="647" height="271" alt="Internet ping" src="https://github.com/user-attachments/assets/3e58dc19-e374-47f5-8be8-4087be78ebcd" />

**DNS:**

<img width="646" height="520" alt="image" src="https://github.com/user-attachments/assets/1b3b5be6-d11f-4356-96c9-13e7fd11428b" />

## 5. Problems I Hit and How I Fixed Them 🚩🛠️

### Problem 1: No internet because the adapter was on NAT instead of NAT Network

#### What I saw
After setting the static IP (10.0.0.2, gateway 10.0.0.1), every ping failed with "Destination Host Unreachable", and the VM had no internet.

<img width="652" height="784" alt="Ping errors" src="https://github.com/user-attachments/assets/580f9042-7cfc-4d7f-8ebf-e5c1a77b2988" />

#### How I investigated
- `ip a` and `ip route` showed my settings had been applied (10.0.0.2/24, default route via 10.0.0.1).
- `ip neigh` showed `10.0.0.1 dev eth0 FAILED`, meaning nothing on the network answered for the gateway address.
- I ran the three `nmcli` commands from the lab slide (the DAD timeout fix), but the problem stayed.
- I switched Kali to automatic (DHCP) addressing to find out which network the VM was really on.

<img width="652" height="352" alt="ip neigh output" src="https://github.com/user-attachments/assets/38e436e3-57e8-4fa4-991f-70dc97665eee" />

#### What the clue was
With DHCP, Kali received **10.0.2.15** with gateway **10.0.2.2**. That is the range VirtualBox uses for plain **NAT**, not my NatNetwork (10.0.0.0/24). I checked the NAT Network settings, and they were correct, which ruled out a wrong subnet. Then I opened the VM's adapter settings and found **Attached to: NAT**.


Output after switching to automatic (DHCP) addressing:

```
inet 10.0.2.15/24 brd 10.0.2.255 scope global dynamic noprefixroute eth0
default via 10.0.2.2 dev eth0 proto dhcp src 10.0.2.15 metric 100
10.0.2.0/24 dev eth0 proto kernel scope link src 10.0.2.15 metric 100
```

<img width="769" height="517" alt="image" src="https://github.com/user-attachments/assets/984eb40e-5dfa-486e-9da9-873161adec74" />



#### The fix
I shut the VM down, changed Adapter 1 to **Attached to: NAT Network** with the name **NatNetwork**, and started it again. My static IP settings were already saved, so I did not need to change them.

#### Result
`ping 10.0.0.1` and `ping 8.8.8.8` both received replies, and the internet worked.

<img width="648" height="508" alt="Working ping after the fix" src="https://github.com/user-attachments/assets/0a035e3b-0437-4cef-bd7a-6b587cc4c6f7" />

#### Lesson
When I see "Destination Host Unreachable" coming from my own IP address, I should first check how the VM's adapter is attached, before changing IP settings.

### Problem 2: Copy and paste not working
Copy and paste between Windows and Kali did nothing. The cause was the VM's Shared Clipboard setting, which was **Disabled**. I changed it to **Bidirectional** and it worked (see Step 5).

## 6. What I Learned
- **Setting up a virtual lab:** I installed VirtualBox, created a Kali VM, and got it running with stable settings.
- **NAT versus NAT Network:** Plain NAT gives each VM its own private connection to the internet (10.0.2.x), while a NAT Network is a shared virtual network where lab machines can reach one another. Creating the network is not enough, because the VM's adapter must also be attached to it.
- **Reading network output:** `ip neigh` showed that the gateway never answered, and the DHCP test showed which network Kali was really on. Testing with automatic addressing was a quick way to find the cause.
- **Static IP configuration:** I configured a static private IPv4 address (10.0.0.2/24) with a gateway and a DNS server.
- **Verifying connectivity:** I used `ping` and `nslookup` to prove that the gateway, the internet, and DNS all work.
- **Shared clipboard:** I learned that guest tools can be running and the feature still fails if the setting on the VirtualBox side is disabled.
- **Snapshots:** I learned that a snapshot gives me a safe point to restore, which matters when testing risky tools.

## 7. Safety and Ethics
This lab is for learning and is isolated: Kali runs inside VirtualBox on a private NAT Network, separate from my main system. I will only test machines I own, my own lab VMs, or deliberately vulnerable training targets that allow it, and I will get written permission before testing anything else. Unauthorised access is illegal even when nothing is damaged, so I will not scan or attack systems outside that scope. The shared clipboard is set to bidirectional for convenience, so before handling any malware samples I will switch it off and tighten the isolation.

## 8. Tools Used
- **VirtualBox 7.2.20:** virtualisation platform for the lab
- **Kali Linux 2026.2:** the guest OS used for security testing
- **Command-line tools:** `ip a`, `ip route`, `ip neigh`, `ping`, `nslookup`
- **`nmcli` (NetworkManager):** applying and checking the network configuration
- **GitHub:** documenting the project

---
**Author:** Delon Omusi Owiti | Networkwalks Cybersecurity Program, Batch B084 | Week 1
