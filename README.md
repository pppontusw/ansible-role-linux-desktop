# Linux Desktop Environment install

## What does it do?

Installs your preferred desktop environment (as long as you prefer one of the following):

gnome
kde
cinnamon
mate
xfce

## What do you have to do?

I recommend you copy the content of the vars folder (all the kde.yml, xfce.yml etc.) to your group_vars and define new groups for each desktop environment.

If you don't want to do that (or you only use one DE always) you can of course specify the variables manually, in the defaults/main file or in your playbook. In that case look at the respective files in vars/* and copy the configuration for the DE you want.

Also make sure you update the defaults/main with the name of your user if you're not using your Ansible user for the desktop tasks.

## How can I use this role?

We can use this role either just to install one desktop environment on selected servers, or to set up different desktop environments depending on host groups. 

This is a simple example on how it would look if I want KDE on all my hosts

```
# example-basic-playbook.yml
# install KDE
- name: basic configuration
  hosts: all 
  vars:
  	linux_desktop: kde
  roles: 
    - linux-desktop
```

``` ansible-playbook example-basic-playbook.yml ```

However the preferred way would be to define your Desktop environment in your group_vars files, by copying the files in vars/* to group_vars we can define our desktop environments through groups in our hosts file:

```
#/etc/ansible/hosts
[linux]
debian1
debian2

[kde]
debian1

[gnome]
debian2
```

```
#group_vars/kde.yml
linux_desktop: kde
linux_boot_to_desktop: true
```

```
#group_vars/gnome.yml
linux_desktop: gnome
```

```
# example-playbook.yml
# sets up basic account and security configuration
- name: basic configuration
  hosts: all 
  roles: 
    - linux-desktop
```

This will set it up so that our servers will get the DE according to which group they are in!


##Compability

Tested on:

Debian 8 & CentOS 7