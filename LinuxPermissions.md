# Linux File Permissions and Group Access Control

I started out by adding three users:
```
hazoor@ICT171Labs:~$ sudo adduser alice
info: Adding user `alice' ...
info: Selecting UID/GID from range 1000 to 59999 ...
info: Adding new group `alice' (1001) ...
info: Adding new user `alice' (1001) with group `alice (1001)' ...
warn: The home directory `/home/alice' already exists.  Not touching this directory.
warn: Warning: The home directory `/home/alice' does not belong to the user you are currently creating.
New password: 
BAD PASSWORD: The password is shorter than 8 characters
Retype new password: 
passwd: password updated successfully
Changing the user information for alice
Enter the new value, or press ENTER for the default
	Full Name []: 
	Room Number []: 
	Work Phone []: 
	Home Phone []: 
	Other []: 
Is the information correct? [Y/n] 
info: Adding new user `alice' to supplemental / extra groups `users' ...
info: Adding user `alice' to group `users' ...
hazoor@ICT171Labs:~$ sudo adduser bob
info: Adding user `bob' ...
info: Selecting UID/GID from range 1000 to 59999 ...
info: Adding new group `bob' (1002) ...
info: Adding new user `bob' (1002) with group `bob (1002)' ...
info: Creating home directory `/home/bob' ...
info: Copying files from `/etc/skel' ...
New password: 
BAD PASSWORD: The password is shorter than 8 characters
Retype new password: 
passwd: password updated successfully
Changing the user information for bob
Enter the new value, or press ENTER for the default
	Full Name []: 
	Room Number []: 
	Work Phone []: 
	Home Phone []: 
	Other []: 
Is the information correct? [Y/n] 
info: Adding new user `bob' to supplemental / extra groups `users' ...
info: Adding user `bob' to group `users' ...
hazoor@ICT171Labs:~$ sudo adduser mallory
info: Adding user `mallory' ...
info: Selecting UID/GID from range 1000 to 59999 ...
info: Adding new group `mallory' (1003) ...
info: Adding new user `mallory' (1003) with group `mallory (1003)' ...
info: Creating home directory `/home/mallory' ...
info: Copying files from `/etc/skel' ...
New password: 
BAD PASSWORD: The password is shorter than 8 characters
Retype new password: 
passwd: password updated successfully
Changing the user information for mallory
Enter the new value, or press ENTER for the default
	Full Name []: 
	Room Number []: 
	Work Phone []: 
	Home Phone []: 
	Other []: 
Is the information correct? [Y/n] 
info: Adding new user `mallory' to supplemental / extra groups `users' ...
info: Adding user `mallory' to group `users' ...
```

I proceeded to create a share group & add alice and bob to the group.
```
hazoor@ICT171Labs:~$ sudo addgroup sharegrp
info: Selecting GID from range 1000 to 59999 ...
info: Adding group `sharegrp' (GID 1004) ...
hazoor@ICT171Labs:~$ sudo usermod -aG sharegrp alice
hazoor@ICT171Labs:~$ sudo usermod -aG sharegrp bob
```

I then created & set up the ownership for shared folder and subfiles.
```
hazoor@ICT171Labs:~$ sudo mkdir /home/share
hazoor@ICT171Labs:~$ sudo touch /home/share/file{1..10}
hazoor@ICT171Labs:~$ sudo chown -R alice /home/share
hazoor@ICT171Labs:~$ sudo chgrp -R sharegrp /home/share
hazoor@ICT171Labs:~$ ls -la /home/share
total 8
drwxr-xr-x 2 alice sharegrp 4096 Oct  1 18:58 .
drwxr-xr-x 8 root  root     4096 Oct  1 18:58 ..
-rw-r--r-- 1 alice sharegrp    0 Oct  1 18:58 file1
-rw-r--r-- 1 alice sharegrp    0 Oct  1 18:58 file10
-rw-r--r-- 1 alice sharegrp    0 Oct  1 18:58 file2
-rw-r--r-- 1 alice sharegrp    0 Oct  1 18:58 file3
-rw-r--r-- 1 alice sharegrp    0 Oct  1 18:58 file4
-rw-r--r-- 1 alice sharegrp    0 Oct  1 18:58 file5
-rw-r--r-- 1 alice sharegrp    0 Oct  1 18:58 file6
-rw-r--r-- 1 alice sharegrp    0 Oct  1 18:58 file7
-rw-r--r-- 1 alice sharegrp    0 Oct  1 18:58 file8
-rw-r--r-- 1 alice sharegrp    0 Oct  1 18:58 file9
```

I proceeded to change the permissions for the folder and files
```
hazoor@ICT171Labs:~$ sudo chmod -R 750 /home/share/
hazoor@ICT171Labs:~$ sudo ls -la /home/share
total 8
drwxr-x--- 2 alice sharegrp 4096 Oct  1 18:58 .
drwxr-xr-x 8 root  root     4096 Oct  1 18:58 ..
-rwxr-x--- 1 alice sharegrp    0 Oct  1 18:58 file1
-rwxr-x--- 1 alice sharegrp    0 Oct  1 18:58 file10
-rwxr-x--- 1 alice sharegrp    0 Oct  1 18:58 file2
-rwxr-x--- 1 alice sharegrp    0 Oct  1 18:58 file3
-rwxr-x--- 1 alice sharegrp    0 Oct  1 18:58 file4
-rwxr-x--- 1 alice sharegrp    0 Oct  1 18:58 file5
-rwxr-x--- 1 alice sharegrp    0 Oct  1 18:58 file6
-rwxr-x--- 1 alice sharegrp    0 Oct  1 18:58 file7
-rwxr-x--- 1 alice sharegrp    0 Oct  1 18:58 file8
-rwxr-x--- 1 alice sharegrp    0 Oct  1 18:58 file9
```

I switched to alice's account to test her access. Alice was able to run everything without errors.
```
hazoor@ICT171Labs:~$ sudo su - alice
alice@ICT171Labs:~$ whoami
alice
alice@ICT171Labs:~$ echo "hello" > /home/share/file1
alice@ICT171Labs:~$ cat /home/share/file1
hello
alice@ICT171Labs:~$ /home/share/file2
```

I switched to bob's account to test his access. Bob was unable to write into any files, but got no error when reading or executing files.
```
hazoor@ICT171Labs:~$ sudo su - bob
bob@ICT171Labs:~$ whoami
bob
bob@ICT171Labs:~$ echo "hi" > /home/share/file2
-bash: /home/share/file2: Permission denied
bob@ICT171Labs:~$ cat /home/share/file1
hello
bob@ICT171Labs:~$ /home/share/file2
```

I switched to mallory's account to test their access. Mallory had no access so got errors at every step.
```
hazoor@ICT171Labs:~$ sudo su - mallory
mallory@ICT171Labs:~$ whoami
mallory
mallory@ICT171Labs:~$ echo "bye" > /home/share/file2
-bash: /home/share/file2: Permission denied
mallory@ICT171Labs:~$ cat /home/share/file1
cat: /home/share/file1: Permission denied
mallory@ICT171Labs:~$ /home/share/file2
-bash: /home/share/file2: Permission denied
```

I added mallory to the sudoers groups, which allowed them to access all permissions like alice via the sudo command.
```
hazoor@ICT171Labs:~$ sudo usermod -aG sudo mallory
hazoor@ICT171Labs:~$ groups mallory
mallory : mallory sudo users
hazoor@ICT171Labs:~$ sudo su - mallory
To run a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.

mallory@ICT171Labs:~$ sudo cat /home/share/file1
[sudo] password for mallory: 
hello
mallory@ICT171Labs:~$ sudo nano /home/share/file1
mallory@ICT171Labs:~$ sudo cat /home/share/file1
#! /bin/bash

echo "bye"
mallory@ICT171Labs:~$ sudo /home/share/file1
bye
```

I proceeded to use mallory's sudo access to delete the folder and its contents
```
mallory@ICT171Labs:~$ sudo rm -r /home/share
mallory@ICT171Labs:~$ ls -l /home/share
ls: cannot access '/home/share': No such file or directory
```

### Reflections

- How do Linux permissions differ from Windows ACL?
    - They are specific to one specific user (the owner), one specific group (“owner group”), and one lastly everyone else on the computer. No additional users or groups can be assigned when it comes to basic permissions.
- What’s the effect of chmod 770 vs 750?
    - `770` allows read, write, and execute access for owning user and group, while allowing no access for everyone else
    - `750` allows read, write, and execute access for the owner along with read & execute access for the group. Like before, no access for everyone else
- What is the risk of adding users to the sudo group?
    - Users with sudo access have effectively full access to the operating system, able to take any actions as long as it is preceded with `sudo`.
- Why is it important to verify with `su` and `whoami`?
    - To make sure you are using the correct account as each account's access to a system is different.