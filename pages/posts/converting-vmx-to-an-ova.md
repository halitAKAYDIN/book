# Converting VMX to an OVA

### Download Ovftool Linux

[Download Releases VMware Ovftool Tool](https://my.vmware.com/group/vmware/downloads/details?downloadGroup=OVFTOOL420&productId=614)

### Install ovftool

```text
chmod +x VMware-ovftool-4.2.0.bundle
sudo ./VMware-ovftool-4.2.0.bundle
```

### Convert File

```text
ovftool vmware-file.vmdk virtualbox-file.ova
```

### Import File

```text
vboxmanage import virtualbox-file.ova --vsys 0 --vmname "Virtual Machine"•
```

### Start Machine

```text
vboxmanage startvm "Virtual Machine"
```

