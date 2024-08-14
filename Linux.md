
###### What is Kernel?
- Kernel is a central model of an operating system which loads first. It reads all the config files during boot.
- After boot it wont read any files until we give an instruction. That's why we restart while installing a new program.
- This kernel is **Linux**. We say linux is open source since the **kernel is an open source**.
- Operating system is bunch of applications bundled with the kernel.
###### What is virtualization?
###### Types of Users:
- **Root ( Admin )**. These are created when the OS is installed and by us. **UID: 0**
- **Guest ( Normal User )**. These are created by the root user. **UID : Starts from 1001.**
- **System User**. These users are created by the operating system. They won't have login but they will own some files directories or a process. **UID : 1 -> 1000.**
###### Syntax of a command
- `command option arguments`.
- ex: `ls -a ./`
###### Terminal
- Terminal is an access interface between user and system.
`[root @ workstation ~]#`
`[user @ workstation ~]$`
   - `user`->The name of the user who is accessing the terminal.
   - `workstation`-> Host.
   - `~`->pwd.
###### File Hierarchy
- `/`->Parent directory or root directory. Doesn't mean the root user.
	- `/bin` -> Normal user commands
	- `/sbin` -> Sudo user commands
	- `/etc` -> Contains all the config files that kernel can read.
	- `/home` -> Contains all the users' directories and files in separate directories. Default home directory of all normal users.
	- `/root` -> Default home directory of root user.
	- `/usr` -> Contains all the information about the commands. Also has the `/bin` directory.
	- `/var` -> Contains all the log files. Error logs are stored.
	- `/tmp` -> Contains temporary folders.
	- `/dev`-?
	- `/opt` -> All third party packages are present


- -rw-r--r--. 1 rohit rohit  0 Aug 12 14:54 file1
	- `.` is trivial entry.
	- `1` means its pointing to the same file itself.
	- rgo permission
	- default for directory - 755
	- default for file -  644

###### Umask

- umask provides basic security for files & directory.
- By default the file permissions are 666
- umask-022 --> 666-022 = 644\
- For directory the default permissions are 777
- umask 022 --> 777-022 = 755
- we can change umask value by `umask [value]`. This is temporary so gets restored until reboot.
- For permanent change, edit /root/.bashrc.
- 

###### VI Editor
3 modes:
    - Insert mode [press i]
    - command mode [press esc]
    - colon mode [ press esc +]
    - to write :w, to quit :q, ! -> forcefully
    - ex: :q! quits forcefully. :w! just saves the file.

Shortcuts:(in command mode)
    - Shift + G -> goto last line
    - G + G -> goto first line
    - :[line_no] -> goes to the line number line_no
    - Y + Y -> copy single line
    - P -> paste
    - [n] + Y + Y -> copies n continuous lines
    - D + D -> Deletes the line
    - [n] + D + D -> Deletes n continuous lines
    - :/[search] -> searches [search], n-> goes downward, Shift+n->goes upwards
        - :nohl -> removes the line highlights
    - :set nu -> sets the line numbers -> set number
    - :set nonu -> sets to the default mode without numbers
    - O -> creates a new line at last

##### User Management
- User : One who owns a file or directory or process

###### Group
- Group : Collection of users with the same permission
    - config file(info about group) -> /etc/group
    - password file -> /etc/gshadow
    - `groupadd [groupname]` adds the group
    - `getent group [groupname]` -> gets the information about the specific group
    - general format `group_name:password_pointer:Group_ID:list_of_users_in_group`
    - `groupmod -n [newgroupname] [oldname]` -> changes the group name
    - `groupmod -g [group_ID] [groupname]` -> changes the group ID
- Types of group:
    - Primary Group : user itself and its mandatory
    - Secondary Group : other users and optional. Can be multiple users

###### User
- config file -> /etc/passwd
- password file -> /etc/shadow
- Every command same as group but change `
- General Format -> UserName:Password_pointer:UID:GID:[comment]:Home_Directory:Shell
    

###### ACL 
- Used to set custom permissions.
- + denotes custom permissions are set to a user or group.
- getfacl 
- `setfacl -m u:username:[rwx] [filename] `-> sets the permission to user.
- `setfacl -m g:groupname:[rwx] [filename] `-> sets the permission to group.
- `setfacl -m [g/u]:groupname:[rwx] -R [directoryname] `-> sets the permission to directories.

###### RPM - Redhat Package Manager

- `rpm -qa` -> lists out all the rpm's installed 
- `rpm -qa | wc -l` -> lists the number of rpm's
- Naming format:
	- name - version . release. architecture
	- ex : vlc-plugin-pipewire-3-2.fc40.x86_64
	- Types of architecture:
		- 64 -> x86_64
		- 32 -> i686
		- no architecture -> noarch
- Contents of RPM:
	- Files to be installed
	- info about rpm (name, version, release)
	- script: (optional)
		- pre script -> runs before installation for ex. warning about architecture support
		- post script -> runs after installation for ex. reboot.
- `rpm -ivh program.rpm` - installs rpm
- `rpm -Uvh program.rpm` - Updates rpm
- `rpm -e program.rpm` - removes rpm
- rpm also extracts but it extracts into different locations based on the properties of files. For ex : config files are extracted to /etc.

###### YUM - Yellowdog Update Manager

- ISO file contains about 7000 RPMs
	- contains AppStream and BaseOS
- `createrepo /.../AppStream` ->  
- `createrepo /.../BaseOS`->
- `vim /etc/yum.repos.d/name.repo`
```vim
	[Appstream] //repo id
	cim name = my 1st repo
	baseurl = file://[location_of_appstream]
	enabled = 1
	gpgcheck = 0
	[BaseOS]
	name = 2nd repo
	baseurl = file://[location_of_baseOS]
	enabled = 1
	gpgcheck = 0
```

###### Archive & Compression

- `tar -cvf filename.tar file1 file2 ... fileN`
- `tar - filename.tar`
- `tar -xf filename.tar` ->
- `du -b filename.tar` -> dtfisk usage
- 3 ways to compress -> gzip, xz, bzip2

- `lsblk` -> list block
- partition editors:
	- fdisk -> MBR (upto 2tb)
		- max 15 partitions or more but initially 4.
		- first 3 partitions are primary and the 4th one is extended with size of the remaining harddisk. The 4th partition is subdivided into further partitions.
	- gdisk -> GPT 
	- parted -> servers
- Two types of partions:
	- primary/active
	- extender/inactive
`partprobe` -> informs all the partition info to the kernel
`udevadm settle`-> same function but for GPT
![[Pasted image 20240813142244.png]]
- 4th partition has 1K as it contains information about the extended partitions.
- fat32 can't handle
- `mkfs.[filesystem] partition name`

###### Mounting

- Assigning a folder to the partition
- Partition --> File system assign --> mount point(directory)
- To mount `mount partition mount_point`
- change the config file `/etc/fstab` so that kernel know the partitions' mount point
- ![[Pasted image 20240813150213.png]]
- first 0 -> no backup
- second 0 -> no serial checking
- after this `mount -a` to permanent mounting
![[Pasted image 20240813161114.png]]
ID type should be changed
fstab --> sile system  table
- `find [location] [filters]`
	- ex: `find /root -size +10M`
si

###### LVM -> Logical Volume Manager
- PV -> optional. It is a id to the volumes
- if all the PVs are attached together it is called Volume Group.
- Using LVM we can decrease the size also.

partition(also change type to lvm) ---> PV `pvcreate [partition]` ---> VG `vgcreate [nameOfVG] [partition]` or if vg already exists `vgextend [nameOfVG] [partition]` or if you need to reduce the vg `vgreduce [nameOfVG] [partition]`f
etc/sysconfig/network-scripts/ifcfg-eth0
