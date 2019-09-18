# multipass-maas
This repository contains cloud-init configuration that will atomatically
install and configure MAAS for testing in Multipass.

# How does it work
Multipass creates a VM where MAAS will be installed and configure. Inside
such VM, KVM will be installed for nested KVM. MAAS will be configured
to use such nested KVM as a KVM host, allowing users to quickly create
new VMs.

The configuration process will configure a separate network for the VMs
to PXE on (192.168.122.0/24) on a separate libvirt network.

After the installation process completes, users will be able to start
creating VMs out of the box.

# Getting started
To install MAAS, it is as simple as running the following command. This
will create a Multipass VM with 10 CPUs, 10 GB of RAM and 40G of storage.

```
 $ multipass launch daily:bionic --name maas -c 10 -m 10G -d 40G --cloud-init maas.cfg
```

After this process is complete, you can now access MAAS.

## Accessing MAAS
To access MAAS we simply need to find out the Multipass VM IP address:

```
 $ multipass info maas
   Name:           maas
   State:          Running
   IPv4:           10.165.99.141
   [...]
```

Using this IP address, we can log into the MAAS UI (http://10.165.99.141:5240/MAAS/).
The default credentials are admin:admin.

## Using MAAS
MAAS is now ready to use, the only manual step you need is to create VMs yourself.
Please refer to `Compose pod virtual machines` under https://maas.io/docs/cli-composable-machines-management
for more information.

# Known issues
In some situations, multipass will timeout and will give the impression
that the installation and setup has failed and a similar message will
be displayed:

```
 $ multipass launch daily:bionic --name maas -c 2 -m 6G -d 20G --cloud-init cloud-init.sh
   launch failed: The following errors occurred:
   timed out waiting for initialization to complete
```

Please note that this is *NOT* and actual failure, given that
cloud-init will continue running the configuration. To check the
progress, please access the multipass instance with:

```
 $ multipass shell maas
```

And tail the logs to confirm cloud-init is still working:

```
 multipass@maas:~$ tail -f /var/log/cloud-init-output.log
```

