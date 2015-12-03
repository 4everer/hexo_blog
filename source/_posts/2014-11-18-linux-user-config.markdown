---
layout: post
title:  "Linux user config"
tags: [linux]
categories: programming
---

*on Ubuntu 12.04*
# **useradd**
refer [this page](https://www.digitalocean.com/community/tutorials/how-to-add-and-delete-users-on-ubuntu-12-04-and-centos-6)

To add a new user in Ubuntu, use the adduser command, replacing the “newuser” with your preferred username.

`sudo adduser newuser`

As soon as you type this command, Ubuntu will automatically start the process:

Type in and confirm your password
Enter in the user’s information. This is not required, pressing enter will automatically fill in the field with the default information
Press Y (or enter) when Ubuntu asks you if the information is correct
Congratulations—you have just added a new user. You can log out of the root user by typing exit and then logging back in with the new username and password.

___

# **How to Grant a User Root Privileges**
As mentioned earlier, you are much better off using a user with root privileges.
You can create the sudo user by opening the sudoers file with this command:
`sudo /usr/sbin/visudo`

Adding the user’s name and the same permissions as root under the the user privilege specification will grant them the sudo privileges.
``` bash
#User privilege specification
root      ALL=(ALL:ALL) ALL
newuser   ALL=(ALL:ALL) ALL
```
Press ‘cntrl x’ to exit the file and then ‘Y’ to save it.

# **Delete a USER Account with UID 0**
You won't be able to delete second root user with another UID 0 using userdel command.
``` bash
userdel john
> $ userdel: user john is currently used by process 1
```

To delete user john with UID 0, open /etc/passwd file and change john's UID.
For example, change the line :
``` bash
> john:x:0:0::/home/john:/bin/sh
```
to something like:
``` bash
> john:x:1111:0::/home/john:/bin/sh
```
Now, you'll be able to delete user john with userdel command: `userdel john`
