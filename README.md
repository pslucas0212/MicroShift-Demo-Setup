# MicroShift Demo Setup

Install RHEL 9.2

Register system
```
$ sudo subscription-manager register --org=xxxxxxxxx --activationkey=your-activation-key
```

Update system to latest code  
```
$ sudo dnf update -y  && sudo dnf upgrade -y
```
Enable AAP 2.4 repo
```
$ sudo subscription-manager repos --enable ansible-automation-platform-2.4-for-rhel-9-x86_64-source-rpms
```

Linux setup
```
$ sudo dnf install python3-pip
```

```
$ python3 -m pip install ansible-navigator --user
```

```
$ sudo dnf install podman
```

Login to redhat.registry.io
```
$ podman login registry.redhat.io
Username: your-userid
Password: 
Login Succeeded!
```  
```
$ ansible-navigator
```

### Appendix
- [Installing ansible-navigator with execution environment support](https://ansible.readthedocs.io/projects/navigator/installation/)
- [How do I install podman in RHEL 8 or 9?](https://access.redhat.com/solutions/3650231)
