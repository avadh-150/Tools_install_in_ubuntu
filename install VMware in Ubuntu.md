first install all dependencies like 
**1**
	sudo apt update
	sudo apt install build-essential gcc make perl -y
	
**2**
	sudo apt install curl git gcc-12 libgcc-12-dev build-essential linux-headers-$(uname -r) -y

**3**       Download the VMware Installer (.bundle file)**

Go to the official VMware site:

**[https://www.vmware.com/products/workstation-pro/workstation-pro-evaluation.html](https://www.vmware.com/products/workstation-pro/workstation-pro-evaluation.html)**

Download one of these:

- **VMware Workstation Pro for Linux (.bundle)**
    
- **VMware Workstation Player for Linux (.bundle)**
    Save it in your Downloads folder.

**4**

look like :  VMware-Workstation-Full-17.6.4-24832109.x86_64.bundle what have i install from 
	https://www.techspot.com/downloads/189-vmware-workstation-for-windows.html

**5** 

		cd Downloads/
		Chmod +x VMware-Workstation-Full-17.6.4-24832109.x86_64.bundle
		sudo sh VMware-Workstation-Full-17.6.4-24832109.x86_64.bundle


and just open the Vmware and it get Ready.




Commands used in this video:

	sudo vmware-modconfig --console --install-all
	sudo apt install build-essential linux-headers-$(uname -r)

1️⃣ Install required packages

	sudo apt update
	sudo apt install mokutil openssl

2️⃣ Create signing key

	mkdir -p ~/vmware-signing
	cd ~/vmware-signing
	openssl req -new -x509 -newkey rsa:2048 -keyout MOK.priv -outform DER -out MOK.der -nodes -days 36500 -subj "/CN=VMware Module Signing/"

3️⃣ Enroll key with Secure Boot (MOK)
	
	sudo mokutil --import MOK.der

👉 Set a password and reboot.

👉 In the blue MOK Manager screen: Enroll MOK → Continue → Yes → Enter password → Reboot

4️⃣ Find VMware module locations

	modinfo -n vmmon
	modinfo -n vmnet

5️⃣ Sign the modules (replace paths if different)

	sudo /usr/src/linux-headers-$(uname -r)/scripts/sign-file sha256 ~/vmware-signing/MOK.priv ~/vmware-signing/MOK.der /lib/modules/6.8.0-87-generic/misc/vmmon.ko
	sudo /usr/src/linux-headers-$(uname -r)/scripts/sign-file sha256 ~/vmware-signing/MOK.priv ~/vmware-signing/MOK.der /lib/modules/6.8.0-87-generic/misc/vmnet.ko

6️⃣ Reload modules

	sudo modprobe -r vmmon vmnet
	sudo modprobe vmmon
	sudo modprobe vmnet

7️⃣ Verify modules are loaded

	lsmod | grep vmmon
	lsmod | grep vmnet

✅ With this fix, VMware will run perfectly on Ubuntu or Lubuntu without turning off Secure Boot.
