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
Update path
```
echo 'export PATH=$HOME/.local/bin:$PATH' >> ~/.profile
source ~./profile
```
Install podman for support of navigator
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

Create a non-root ansible user that can sudo
```
# useradd ansible
# passwd ansible
Changing password for user ansible.
New password: 
Retype new password: 
passwd: all authentication tokens updated successfully.
```

Add ansible user to sudo account  
1. As root, open the /etc/sudoers file.
```
# visudo
```
2. In the /etc/sudoers file, find the lines that grant sudo access to users in the administrative wheel group.
```
## Allows people in group wheel to run all commands
%wheel        ALL=(ALL)       ALL
```
3. Add users you want to grant sudo access to into the administrative wheel group.
```
# usermod --append -G wheel <username>
```
4. Verify that the user is added to the administrative wheel group:
```
# id ansible
uid=1001(ansible) gid=1001(ansible) groups=1001(ansible),10(wheel)
```



### Appendix
- [Installing ansible-navigator with execution environment support](https://ansible.readthedocs.io/projects/navigator/installation/)
- [How do I install podman in RHEL 8 or 9?](https://access.redhat.com/solutions/3650231)
