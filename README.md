# Juniper vMX on KVM

Tutorial for installing the Juniper vMX on KVM

## Getting Started

These instructions will get you a copy of the project up and running on your local machine for development and testing purposes. See deployment for notes on how to deploy the project on a live system.

### Prerequisites

What things you need to install the software and how to install them

```
VM with CentOS 7.2
	 8GB RAM, 4 vCPUs and two vNICs
	 Also the VM is enabled to support hypervisor applications within the VM.
vMX.ova images
	default username/pass: root/root123
```

### Installing

#### Part1: KVM installation and preparation

At this point CentOS Server has been freshly installed, and the option to install virtualisation was selected during setup.

Make sure that your system has the hardware virtualization extensions: For Intel-based hosts, verify the CPU virtualization extension [vmx] are available using following command.
```
grep -e 'vmx' /proc/cpuinfo
```

For AMD-based hosts, verify the CPU virtualization extension [svm] are available.
```
grep -e 'svm' /proc/cpuinfo
```

If there is no output make sure that virtualization extensions is enabled in BIOS. Verify that KVM modules are loaded in the kernel “it should be loaded by default”.
```
lsmod | grep kvm
```

Before starting , you will need the root account or non-root user with sudo privileges configured on your system and also make sure that your system is up-to-date.
```
yum update
```

Make sure that Selinux be in Permissive mode.
```
setenforce 0
```

We will install qemu-kvm and qemu-img packages at first. These packages provide the user-level KVM and disk image manager.
```
yum install -y qemu-kvm qemu-img
```

Now, you have the minimum requirement to deploy virtual platform on your host, but we also still have useful tools to administrate our platform such as:
```
virt-manager provides a GUI tool to administrate your virtual machines.
libvirt-client provides a CL tool to administrate your virtual environment this tool called virsh.
virt-install provides the command “virt-install” to create your virtual machines from CLI.
libvirt provides the server and host side libraries for interacting with hypervisors and host systems.
Let’s install these above tools using the following command.
```

```
yum install -y virt-manager libvirt libvirt-python libvirt-client 
```

For RHEL/CentOS7 users, also still having additional package groups such as: Virtualization Client, Virtualization Platform and Virtualization Tools to install.
```
yum groupinstall virtualization-client virtualization-platform virtualization-tools
```

The virtualization daemon which manage all of the platform is “libvirtd”. lets restart it.
```
systemctl restart libvirtd
```

After restarting the daemon, then check its status by running following command.
```
systemctl status libvirtd 
```

Now, lets switch to the next section to create our virtual machines.

#### Part2: Converting OVA for use with KVM / QCOW2

The OVA file is nothing more than a TAR archive, containing the .OVF and .VMDK files. Easy!
```
file vMX.ova
```

I'ts possible to use the tar command to list the contents
```
tar -tf vMX.ova 
```

Simply extract those things...
```
tar -xvf vMX.ova 
```

Now take a look at the created files The OVF XML file describes the image, it makes for some interesting reading about the expectations of the running environment.
```
file vMX* 
```

Recent versions of qemu are able to run directly from the VMDK file, buy why do that? Use QCOW2, it's better. Execute: qemu-img -h and the last line of output shows the supported formats.
```
qemu-img -h |tail -n1
```

Supported formats: raw cow qcow vdi vmdk cloop dmg bochs vpc vvfat qcow2 parallels nbd blkdebug sheepdog host_cdrom host_floppy host_device file
Now actually convert it, this may take some time.
```
qemu-img convert -O qcow2 vMX.vmdk vMX.qcow2
```

#### Part3: Create vMX VM using KVM
Copy the vMX image to the libvirt directory and rename it with the name of your VM
```
cp vMX.qcow2 /var/lib/libvirt/images/vMX.qcow2
```

Install the vMX VM using the virt-install command. You must specify the VM name, memory, and image location. The amount of memory depends on your deployment.
```
virt-install 	--name vMX-VM1 \
				--ram 1024 \
				--disk path=/var/lib/libvirt/images/vMX-VM1.qcow2 \
				--cpu host --vcpus 2 \
				--os-variant none \
				--graphics none \
				--import \
				--network bridge=bridge1,mac=00:05:86:71:92:08,model=e1000 \
				--network network=lan1,mac=00:05:86:71:92:09,model=e1000 \
				--network network=lan2,mac=00:05:86:71:92:00,model=e1000 \
				--network network=lan2,mac=00:05:86:71:92:01,model=e1000 \
				--network network=lan3,mac=00:05:86:71:92:02,model=e1000 \
				--network network=lan3,mac=00:05:86:71:92:03,model=e1000 \
				--network network=lan4,mac=00:05:86:71:92:04,model=e1000 \
				--network network=lan4,mac=00:05:86:71:92:05,model=e1000 \
				--network network=lan5,mac=00:05:86:71:92:06,model=e1000 \
				--network network=lan5,mac=00:05:86:71:92:07,model=e1000 
```

When the installation is completed, the login prompt appears:
```
Amnesiac (ttyd0)

login:
```

To disconnect from the console, press Ctrl + ].
```
press Ctrl + ]
```

Connect to the VM console using the virsh console vm-name command.
```
virsh console vMX-VM1
```

Log in from the console with the username root and password root123. Type cli to access the Junos OS CLI.

Verify that your VM is installed using the show interfaces terse command. The added interfaces appear as em interfaces.

## Source

* Juniper official website
* matt.dinham.net
* techmint.com

