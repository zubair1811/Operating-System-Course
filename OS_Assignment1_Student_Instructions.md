# Operating Systems: Assignment 1 - Setting Up VMs and SSH Networking

## Introduction
This guide provides step-by-step instructions for students to create two virtual machines (Ubuntu and CentOS) in a hypervisor (like Oracle VirtualBox), configure their networking, and establish secure SSH communication between the host machine and the VMs, as well as between the VMs themselves.

> [!WARNING]
> **Important Note for macOS Users:** If you are using a Mac (especially with an M1/M2/M3 chip), you may encounter specific compatibility and permission issues with VirtualBox. Please thoroughly read the **"Troubleshooting for macOS Users"** section at the bottom of this document before beginning.

## Prerequisites
- **Oracle VirtualBox** installed on your host OS.
- **Ubuntu OS ISO file** downloaded.
- **CentOS OS ISO file** (or equivalent Red Hat-based Linux distribution) downloaded.

---

## Part 1: Setting up the Virtual Machines

### Step 1: Prepare Your Environment
Ensure you have downloaded the necessary ISO disk images for the operating systems. Launch VirtualBox to begin creating the virtual machines.

### Step 2: Create the Ubuntu VM
1. Open VirtualBox and click the **"New"** button.
2. Provide a name for the VM.
3. Select the Ubuntu ISO file you downloaded to start the automated installation process.

### Step 3: Allocate Resources for Ubuntu
To prevent system lag, allocate appropriate resources:
- Assign adequate base memory (RAM).
- Assign processor cores according to your host machine's physical hardware limits.

### Step 4: Boot and Install Ubuntu
Start the Ubuntu virtual machine. The system will load the Ubuntu kernel and display the initial boot screen. Follow the on-screen prompts to complete the formal OS installation.

### Step 5: Access the Command Line in Ubuntu
Once Ubuntu is fully installed and booted, launch the **Terminal**. This terminal will be used throughout the assignment to run commands, check configurations, and initiate SSH sessions.

---

### Step 6: Create the CentOS VM
1. In VirtualBox, click **"New"** again to start the creation process for the second virtual machine.
2. Provide a suitable VM name, select the appropriate Red Hat-based Linux version, and target the downloaded CentOS ISO file.

### Step 7: Configure the CentOS VM
Configure the initial settings:
- Customize the storage sizing.
- Set up user credentials (username and password). 
*Note: Setting these correctly is critical for logging in and administering the CentOS server later.*

---

## Part 2: Configuring the Network

### Step 8: Configure the NAT Network in VirtualBox
To allow both VMs to communicate with each other on the same secure subnet:
1. Open the **VirtualBox Preferences**.
2. Navigate to Network and create a new **NAT Network**. This acts like a virtual router bridging the VMs together.
3. In both the Ubuntu and CentOS VM settings, navigate to "Network" and ensure they are attached to this new NAT Network.

### Step 9 & 10: Verify Host Networking
1. Open up your host operating system's terminal (e.g., macOS Terminal or Windows Command Prompt/PowerShell).
2. Check your local IP address configuration (using `ifconfig` on macOS/Linux or `ipconfig` on Windows).
Verify these details to ensure there are no conflicting IP addresses that might interfere with VirtualBox's NAT Network routing.

### Step 11: Check the Ubuntu VM IP Address
Inside your Ubuntu VM terminal, identify its IP address by executing:
```bash
ip a
```
The output will show that the NAT Network successfully assigned an IP address. Note down this IP address.

---

## Part 3: Establishing SSH Connections

### Step 12: Install OpenSSH on Ubuntu
To allow Ubuntu to accept and initiate encrypted remote connections, use the `apt` package manager. Run the following command in the Ubuntu terminal:
```bash
sudo apt update
sudo apt install openssh-server openssh-client
```

### Step 13: Connect from Host PC to Ubuntu VM
Test your setup by connecting to the Ubuntu VM from the host PC over SSH:
1. Open your host PC's terminal.
2. Securely connect using the Ubuntu VM's IP address:
```bash
ssh username@<ubuntu-ip-address>
```
*(Replace `username` with your Ubuntu user name, and `<ubuntu-ip-address>` with the IP found in Step 11).*

### Step 14: Check Ubuntu SSH Status
To double-check the server's health on Ubuntu, run:
```bash
sudo systemctl status ssh
```
An `active (running)` status in green confirms that the OpenSSH background service is operating securely without crashes.

---

### Step 15: Install OpenSSH on CentOS
Switch over to your CentOS VM terminal. Because CentOS relies on the `yum` package manager, you will install OpenSSH utilities differently:
```bash
sudo yum -y install openssh-server openssh-clients
```

### Step 16: Start the SSH Service on CentOS
After installation, you must explicitly enable and start the service so CentOS will listen for incoming SSH connections:
```bash
sudo systemctl enable sshd
sudo systemctl start sshd
```

### Step 17: Get the CentOS IP Address
Find out exactly where CentOS is on the network:
```bash
ip a
```
Write down the provided IPv4 address. It will confirm that CentOS is part of the same VirtualBox NAT network as your Ubuntu machine.

---

## Part 4: Testing VM-to-VM Communication

### Step 18: Connect from Ubuntu to CentOS
Now fulfill the final "Between VMs" assignment requirement:
1. Return to the Ubuntu terminal. 
2. Direct an SSH command toward the CentOS machine:
```bash
ssh username@<centos-ip-address>
```
*(Replace `username` with your CentOS user, and `<centos-ip-address>` with the IP noted in Step 17).*
Enter the correct CentOS password when prompted to log in remotely.

### Step 19: Verify Communication Between VMs
With the connection established, provide definitive proof of administrative access. Run basic Linux file manipulation commands remotely over the connection to alter the CentOS filesystem, such as:
```bash
touch hello_from_ubuntu.txt
ls -l
```
This final action demonstrates full control over the network link and completes the practical setup.

---

## Troubleshooting for macOS Users

If you are a student utilizing a Mac device, you are likely to run into one of the following issues when completing this assignment. Please read through these common problems and their solutions.

### 1. Apple Silicon (M1/M2/M3 chips) Compatibility Issues
**Problem:** Oracle VirtualBox historically relies on x86_64 architecture (Intel chips). If you have a newer Apple Silicon Mac, you will get installation errors or your VMs will fail to boot stating an architecture mismatch.
**Solution:** 
- **Option A (Recommended):** Instead of VirtualBox, use an Apple Silicon native hypervisor such as **[UTM](https://mac.getutm.app/)** or **Parallels Desktop**.
- **Option B:** Make sure you are strictly downloading the **ARM64 (AArch64)** versions of the Ubuntu and CentOS ISOs instead of the AMD64 versions, and ensure you use the "Developer Preview" version of VirtualBox for macOS/ARM64.

### 2. Kernel Extension / Hypervisor Blocked by Privacy Settings
**Problem:** During the VirtualBox installation on macOS (especially Intel Macs on newer macOS versions like Sonoma/Ventura), the installation might "fail" or the VM will refuse to start because the OS blocked the system extension.
**Solution:**
1. Open your Mac's **System Settings** (or System Preferences).
2. Navigate to **Privacy & Security**.
3. Scroll down to the **Security** section. You should see a message stating that system software from developer "Oracle America, Inc." was blocked.
4. Click **Allow**, authenticate with your Mac password, and restart your computer to apply the change.

### 3. VirtualBox NAT Network Fails to Assign IP Address
**Problem:** macOS has a known networking quirk with VirtualBox where the NAT Network DHCP server fails to start, meaning your Ubuntu/CentOS VMs won't get an IP address when you run `ip a` (it will remain empty).
**Solution:**
- Go to the VirtualBox Preferences `->` Network `->` edit your NAT Network, and verify "Support DHCP" is checked.
- If it still doesn't work, switch your VM's Network Adapter setting from "NAT Network" to **"Bridged Adapter"**. This bypasses VirtualBox's internal router and bridges the VM directly to your home Wi-Fi network, allowing it to easily grab an IP address and communicate with both your host Mac and the other VMs.

### 4. "REMOTE HOST IDENTIFICATION HAS CHANGED" SSH Error
**Problem:** When connecting from your Mac's terminal to the VM (`ssh username@<ip-address>`), you might suddenly receive a large security warning block stating that the ECDSA host key has changed. This happens when you delete and recreate a VM that ends up utilizing the same previous IP address.
**Solution:** 
macOS saves the SSH signatures to protect you. You must reset the signature for that IP address by running the following command on your Mac's terminal:
```bash
ssh-keygen -R <ip-address-of-your-vm>
```
After executing this, you can safely connect again.
