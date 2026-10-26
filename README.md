

<h1>🐍⚙️KALI LINUX LAB ENVIRONMENT SETUP<h1/>


<h3>Creating a virtual lab in VirtualBox to test malware, networks, vulnerability assessment, and understand how other operating systems work, e.g Kali Linux </h3>

---

## 1. Goal🥅
To provide a safe, isolated, and pre-configured environment for penetration testing, ethical hacking, digital forensics, and vulnerability assessment.
1. Safety and Isolation - test scripts in a safe place
2. Pre-Loaded Security Toolkit - use hundreds of tools at my disposal
3. Snapshots and Easy Recovery - restoration to safe points
4. Portability and Low Resource Needs - takes less space and is less resource-intensive

## 2. My Lab at a Glance
| Item | My setting |
|------|------------|
| Host OS | Kali Linux|
| VirtualBox version |VirtualBox 7.2.20  |
| Kali version | 2026.2 |
| Network type and name | NAT (NatNetwork)|
| Network range | 10.0.0.1-10.0.0.254 |
| Kali IP / gateway / DNS | 10.0.0.2 / 10.0.0.1 / 8.8.8.8 |

## 3. How I Built It
<!-- One short step each. Say what you did, why, and add a screenshot. -->

### Step 1:VirtualBox Installation
Downloaded the VirtualBox application from the main website, followed all installation instructions, and got the Kali sign-in window.
<img width="960" height="822" alt="image" src="https://github.com/user-attachments/assets/83478bc0-57bc-4fe2-aac1-9708257562ae" />

Tweaked the settings for the display, cpu and memory to make the virtual machine stable.
<img width="1919" height="1031" alt="image" src="https://github.com/user-attachments/assets/73309d0f-4ec4-474b-aa2f-4ac3045c33a9" />

Successfully got the sign-in
<img width="960" height="1032" alt="image" src="https://github.com/user-attachments/assets/30b430c1-903a-43b8-95a3-5d5274a97363" />

### Step 2: Creating the NAT Network
Set the network to NAT
<img width="961" height="1028" alt="Screenshot 2026-10-26 042100" src="https://github.com/user-attachments/assets/69d2031d-121b-493f-b9d9-5f08adc4722a" />

### Step 3: Importing Kali and attaching the adapter
Changed the network in adapter 1 from NAT to NatNetwork
<img width="781" height="518" alt="image" src="https://github.com/user-attachments/assets/6661cf6a-b9e3-4dd6-a26c-68a82ac291b0" />

### Step 4: Setting the IP address
Set the IP address of the Kali machine to 10.0.0.2, the default gateway to 10.0.0.1, and DNS to 8.8.8.8
<img width="698" height="547" alt="image" src="https://github.com/user-attachments/assets/ac02adf0-aea0-48f2-ad27-16ac34d11699" />

### Step 5: shared clipboard / guest tools/snapshot
I tried to copy and paste text from the host and virtual machine and vice versa, but it was not working, so I resorted to tweaking the settings to allow bidirectional copy and paste, which was successful.
<img width="781" height="514" alt="image" src="https://github.com/user-attachments/assets/e41655c4-d737-4f92-8254-8c11ccb23566" />



## 4. Proof It Works
<!-- Commands you ran and a screenshot of each result -->

| Test | Command | What I saw |
|------|---------|------------|
| My IP | `ip a` | 10.0.0.2|
<img width="644" height="515" alt="image" src="https://github.com/user-attachments/assets/98efd0d9-20c4-4bd9-b6af-78dff12ad124" />

| Gateway | `ping ...` | 10.0.0.1 |
<img width="651" height="521" alt="image" src="https://github.com/user-attachments/assets/f4d439a3-80e0-4af1-9eef-776626e875c0" />

| Internet | `ping ...` | 8.8.8.8 |
<img width="647" height="271" alt="image" src="https://github.com/user-attachments/assets/3e58dc19-e374-47f5-8be8-4087be78ebcd" />


| DNS | `nslookup ...` | 8.8.8.8|
<img width="646" height="378" alt="image" src="https://github.com/user-attachments/assets/83a5965a-21de-49c3-badb-8e45bfc09d53" />


## 5. Problems I Hit and How I Fixed Them
<!-- Repeat this block for each problem -->
When I first set the IP as per the instructions, the internet was not working, and the VM couldn't ping its gateway address.
<img width="652" height="784" alt="Screenshot 2026-10-25 182618" src="https://github.com/user-attachments/assets/59e24ea7-954b-4c71-a497-21e4d20625ca" />



### Problem: [short name]
- <h3>What I saw:<h3/>
    The destination was unreachable; no internet connectivity
- **How I investigated:**
-  used ip a, ip route, ip neigh
- **What the clue was:**
- when I ran ip neigh, it located the failure; it showed 10.0.0.1 dev eth0 FAILED
- Kali had asked "who has 10.0.0.1" on my local network and no one answered.
- Together with the “Destination Host Unreachable” replies coming from 10.0.0.2 (my own address), this meant the gateway wasn’t on the network Kali was actually plugged into. At this stage, the problem could still have been a wrong subnet, a wrong gateway, or a wrong attachment.
- **The fix:** [what I changed]
- **Result:** [how I confirmed it worked]

## 6. What I Learned
<!-- 3 to 5 points, each in your own words. Say why it matters, not just what it is. -->
- [...]
- [...]
- [...]

## 7. Safety and Ethics
<!-- One or two sentences: who and what you may test, and that this lab is for systems you own or have permission to test -->
[...]

## 8. Tools Used
- [tool and link]

---
**Author:** [Your name] | Networkwalks Cybersecurity Program, Batch [your batch] | Week 1
