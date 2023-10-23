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

Test Ansible Navigator
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
2. In the /etc/sudoers file, find the lines that grant sudo access to users in the administrative wheel group.  Make sure the group is uncommented (remove # if it start the line).
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
Install Github cli tool
```
$ sudo dnf install 'dnf-command(config-manager)'
$ sudo dnf config-manager --add-repo https://cli.github.com/packages/rpm/gh-cli.repo
$ sudo dnf install gh
Updating Subscription Management repositories.
packages for the GitHub CLI                                                                         7.1 kB/s | 2.6 kB     00:00    
Last metadata expiration check: 0:00:01 ago on Thu 19 Oct 2023 02:59:12 PM CDT.
Dependencies resolved.
...

Complete!
```
Download Device Edge Workshops Github repo.
1. Create and set peronal access token.  See this link for setting up personal access token - [Managing peronsal access tokens](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens#).  Set GH_KEY variable 
```
$ export GH_TOKEN=addY0urT0kenH3re
```
2. Download repo
```
$ gh repo clone redhat-manufacturing/device-edge-workshops
```

If you need to update the repo, go to the ... direcotory and the sync command..
```
$ gh sync repo
```

Add your AWS credentials to your environment.  Permanently persist add them to your ```~/.bash_rc``` file.  You get the keys from your RHPDS open environment.
```
export AWS_ACCESS_KEY_ID=YOURACCESSKEYHERE
export AWS_SECRET_ACCESS_KEY=YouRS3dh4TKEYHeRE
```

Update the extra-vars.yml file.   
1. 

Run playbook from folder/directory where the local-inventory.ym and extra-vars.yml are located.
```
$ ansible-navigator run provisioner/provision_lab.yml --inventory local-inventory.yml --extra-vars @extra-vars.yml -vv -m stdout
```
### Appendix
- [Installing ansible-navigator with execution environment support](https://ansible.readthedocs.io/projects/navigator/installation/)
- [How do I install podman in RHEL 8 or 9?](https://access.redhat.com/solutions/3650231)
- [Red Hat Device Edge Workshop Provisioner](https://github.com/redhat-manufacturing/device-edge-workshops/tree/main/provisioner)
