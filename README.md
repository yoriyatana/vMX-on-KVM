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

####Part1: KVM installation and preparation

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

virt-manager provides a GUI tool to administrate your virtual machines.
libvirt-client provides a CL tool to administrate your virtual environment this tool called virsh.
virt-install provides the command “virt-install” to create your virtual machines from CLI.
libvirt provides the server and host side libraries for interacting with hypervisors and host systems.
Let’s install these above tools using the following command.
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



















```
sudo yum update
yum -y install qemu-kvm libvirt virt-install bridge-utils

```

And repeat

```
until finished
```

End with an example of getting some data out of the system or using it for a little demo

## Running the tests

Explain how to run the automated tests for this system

### Break down into end to end tests

Explain what these tests test and why

```
Give an example
```

### And coding style tests

Explain what these tests test and why

```
Give an example
```

## Deployment

Add additional notes about how to deploy this on a live system

## Built With

* [Dropwizard](http://www.dropwizard.io/1.0.2/docs/) - The web framework used
* [Maven](https://maven.apache.org/) - Dependency Management
* [ROME](https://rometools.github.io/rome/) - Used to generate RSS Feeds

## Contributing

Please read [CONTRIBUTING.md](https://gist.github.com/PurpleBooth/b24679402957c63ec426) for details on our code of conduct, and the process for submitting pull requests to us.

## Versioning

We use [SemVer](http://semver.org/) for versioning. For the versions available, see the [tags on this repository](https://github.com/your/project/tags). 

## Authors

* **Billie Thompson** - *Initial work* - [PurpleBooth](https://github.com/PurpleBooth)

See also the list of [contributors](https://github.com/your/project/contributors) who participated in this project.

## License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details

## Acknowledgments

* Hat tip to anyone who's code was used
* Inspiration
* etc

