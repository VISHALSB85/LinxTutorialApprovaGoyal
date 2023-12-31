Contribute notes based on [this](https://www.youtube.com/watch?v=hxNFeL2qY-k&list=PL2kSRH_DmWVZp_cu6MMPWkgYh7GZVFS6i&index=4) video now!

<br>
<br>

# Content

- [Users-and-permissions](#users-and-permissions)
- [What are users and groups?](#what-are-users-and-groups)
- [Managing users](#managing-users)
  - To create a new Linux user
  - To modify a Linux user
  - Accessing a user configuration file
  - To modify a Linux user
  - Change the user ID for a user
  - Change the user login name
  - To delete a Linux user
- [Manage groups](#manage-groups)
  - To create a group
  - Modify the group ID of a user
- [Superuser access](#superuser-access)
- [File permissions & ownership](#file-permissions--ownership)
  - To change permissions on a file or directory
  - change ownership of a file/directory
  - Example
- [How to manage permissions?](#how-to-manage-permissions)
  - How to use absolute mode?
  - How do I use symbolic mode?
- [Sticky bit](#sticky-bit)

<br>
<br>

# Users-and-permissions

Linux, being a truly multi-user, multi-namespace OS, offers a lot of options when it comes to user management

### What are users and groups?

A user is an entity, in a Linux operating system, that can manipulate files and perform several other operations. Each user is assigned an ID that is unique for each user in the operating system.

After installation of the operating system, the **ID 0 is assigned to the root user** and the IDs 1 to 999 (both inclusive) are assigned to the system users and hence the ids for local user begins from 1000 onwards.

In a single directory, we can create 60,000 users.

A multi-user OS means that Linux can be used by multiple users _at the same time_.
Most Linux distributions have much more users, each one responsible for a certain aspect of the system.

You can not easily log in to them, but you can run commands from their name and set permissions. Groups, then, are just groups of users who need access to a certain resource.

To list all users on your system, run this command:

```
$ less /etc/passwd
```

`/etc/passwd` is the file that holds the information about users, and `less` outputs it nicely.

```
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
irc:x:39:39:ircd:/run/ircd:/usr/sbin/nologin
gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
systemd-network:x:100:102:systemd Network Management,,,:/run/systemd:/usr/sbin/nologin
systemd-resolve:x:101:103:systemd Resolver,,,:/run/systemd:/usr/sbin/nologin
systemd-timesync:x:102:104:systemd Time Synchronization,,,:/run/systemd:/usr/sbin/nologin
messagebus:x:103:106::/nonexistent:/usr/sbin/nologin
```

The format of the `passwd` file is the following:

- username
- password (replaced with x and stored in `/etc/shadow`)
- UID (user id, a number)
- GID (group id, also a number)
- user’s full name
- user’s home directory
- user’s login shell (the program that runs when you log in. The `nologin` is a program that does nothing, used to prevent logging in as system users)

### Managing users

**To create a new Linux user**: use the `useradd` command like this:

```
$ sudo useradd <name>
```

The user is given the ID automatically depending on which category it falls in. The username of the user will be as provided by us in the command.

Additionally, `useradd` can take these arguments:

- `-d <home directory>` – sets user’s home directory
- `-s <shell>` – sets user’s login shell
- `-g <group>` – sets user’s primary group (more on that later)
- `-u <uid>` – sets user’s UID (will be autogenerated by default)

After you have created the user, you need to set a password for him. Unlike windows, if a user does not have a password, there is no way of logging in as him. To set the password run this command:

```
# passwd <username>
```

You will be prompted for the password twice.

**To modify a Linux user:** use the `usermod` command like this:

```
# usermod <username>
```

Like `useradd`, `usermod` will accept the same arguments to set the fields of the user.

**Accessing a user configuration file**: use the `/etc/passwd` command like :

```
cat /etc/passwd
```

This commands prints the data of the configuration file. This file contains information about the user in the format.

```
username : x : user id : user group id : : /home/username : /bin/bash
```

```
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
irc:x:39:39:ircd:/run/ircd:/usr/sbin/nologin
gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin
```

**To modify a Linux user:** use the `usermod` command like this:

```
# usermod <username>
```

Like `useradd`, `usermod` will accept the same arguments to set the fields of the user.

**Change the user ID for a user**:usr   `usermod ` command like this:

```
usermod  -u new_id username
```

This command can change the user ID of a user. The user with the given username will be assigned with the new ID given in the command and the old ID will be removed.

**Change the user login name** using `usermod` command

```
sudo usermod -l new_login_name old_login_name
```

This command is used to change the login name of the user. The old login name of the user is changed to the new login name provided.

**To delete a Linux user:** use the `userdel` command like this:

```
# userdel <username>
```

It accepts an argument `-r` to also delete the user’s home directory and mail.

### Manage groups

Like users, you can create, modify, and delete groups. To view the group membership for a user, as well as its UID, use:

```
$ id <username>
```

Like users, you can list all groups on your setup:

```
$ cat /etc/group
```

**To create a group:** use the `groupadd` command like this:

```
# groupadd <groupname>
```

`groupadd` takes these arguments:

- `-g <GID>` set the group id
- `-f` force the command to return successfully even if group already exists

After the group is created, you might want to add some users to it. The `gpasswd` tool can do that:

```
# gpasswd <username> <groupname>
```

And, lastly, you can edit groups with `groupmod` (same arguments as `groupadd`) and delete groups with `groupdel`.

**Modify the group ID of a user**.

```
usermod -g  new_group_id username
```

This command can change the group ID of a user and hence it can even be used to move a user to an already existing group. It will change the group ID of the user whose username is given and sets the group ID as the given new_group_id.

### Superuser access

`root`, or superuser, is a special user in the system, with unlimited power. `root` can read, write, and execute every single file.
So some distributions disable `root` altogether. Instead, you can use the `sudo` command to run a specific program _as_ `root`.

```
$ sudo apt-get install cowsay
```

Here, the `apt-get install cowsay` command will be run as `root`, after you enter your password. Access to `sudo` is governed by the `/etc/sudoers` file. In it, you will find lines like these:

```
Defaults env_reset

username ALL=(ALL:ALL) ALL
%sudo ALL=(ALL:ALL) ALL
```

The `Defaults env_reset` line clears all environmental variables. This is a safety precaution. Then, the `username ALL=(ALL:ALL) ALL` command does this:

- Let `username`
- On `ALL` hosts
- Run commands as `ALL` users and `ALL` groups
- `ALL` commands are allowed

So, the syntax goes like this:

```
<username> <allowed hosts>=(<allowed users>:<allowed groups>) <allowed_commands>
```

The next line starts with a `%` sign. That means that that rule applies to any user in the `sudo` group. This is convenient because you do not have to edit this file when creating new users, instead, you can add them to `sudo` group.

Now, there may be cases when you need to login as genuine `root`. The `su` command switches current user to `root`, and if you run it with `sudo`, you will be able to switch to `root` using your own password:

```
$ whoami          // michael
$ sudo su
# whoami          // root
```

When logged in as `root`, you can simply run `passwd` to change `root`‘s password and be able to login to it directly **(please do not do this)**.

### File permissions & ownership

Every file and every folder on your system has permission, as well as ownership info. The permissions have these values:

- `0` or `---` – nothing allowed
- `1` or `--x` – execution allowed
- `2` or `-w-` – writing allowed
- `3` or `-wx` – writing and execution allowed
- `4` or `r--` – reading allowed
- `5` or `r-x` – reading and execution allowed
- `6` or `rw-` – reading and writing allowed
- `7` or `rwx` – everything allowed

In addition, every file and directory has an _owner_. The owner is the user that has absolute control over the file. The owner can also be specified as a group. Thus, every file has 3 different permissions: for owner, for owning group, and for everyone else. You can easily view permissions by running `ls -al`:

```
total 128
drwxr-xr-x  20 root root  4096 Nov 13 00:02 .
drwxr-xr-x  20 root root  4096 Nov 13 00:02 ..
lrwxrwxrwx   1 root root     7 Mar 30  2021 bin -> usr/bin
drwxr-xr-x   3 root root  4096 Nov 11 14:22 boot
drwxr-xr-x  20 root root  4800 Nov 23 09:54 dev
-rw-r--r--   1 root root  1424 Nov 10 12:07 docker-desktop.install
-rw-r--r--   1 root root  3059 Nov 10 12:07 docker-desktop.spec
drwxr-xr-x 160 root root 12288 Nov 22 09:38 etc
drwxr-xr-x   3 root root  4096 Oct 31  2021 home
lrwxrwxrwx   1 root root     7 Mar 30  2021 lib -> usr/lib
lrwxrwxrwx   1 root root     9 Mar 30  2021 lib32 -> usr/lib32
lrwxrwxrwx   1 root root     9 Mar 30  2021 lib64 -> usr/lib64
lrwxrwxrwx   1 root root    10 Mar 30  2021 libx32 -> usr/libx32
drwx------   2 root root 16384 Oct 31  2021 lost+found
drwxr-xr-x   3 root root  4096 Nov 15  2021 media
drwxr-xr-x   2 root root  4096 Mar 30  2021 mnt
drwxr-xr-x   5 root root  4096 Nov 13 00:02 opt
-rw-r--r--   1 root root  1028 Nov 10 12:07 PKGBUILD
dr-xr-xr-x 443 root root     0 Nov 23 09:53 proc
drwx------  10 root root  4096 Nov 17 12:36 root
drwxr-xr-x  45 root root  1300 Nov 23 13:45 run
lrwxrwxrwx   1 root root     8 Mar 30  2021 sbin -> usr/sbin
drwxr-xr-x  13 root root  4096 Aug 11 10:22 snap
drwxr-xr-x   2 root root  4096 Mar 30  2021 srv
dr-xr-xr-x  13 root root     0 Nov 23 09:53 sys
drwxr-xr-x   9 root root  4096 Nov 22 20:00 timeshift
drwxrwxrwt  40 root root 36864 Nov 23 19:14 tmp
drwxr-xr-x  14 root root  4096 Sep 20  2021 usr
drwxr-xr-x  15 root root  4096 Aug 14 14:00 var
```

In the first column you can see the permissions, specified in the order `user` `group` `everyone`. For example, only `root` can edit the `boot` directory, but anyone can read and execute stuff from it.

**To change permissions on a file or directory**, use the `chmod` command:

```
$ chmod [options] [mode] [files]
```

Most common `chmod` option is `-R`. It stands for recursive and means that the rule will be applied to all children in a folder.

You can use the `chown` command to **change ownership of a file/directory**. Its syntax is similar:

```
$ chown [options] [user:group] [files]
```

It takes the same option `-R`, which does the same thing.

examples:

```
$ chown -R mike /home/mike
Set ownership to mike for all files in his home directory
```

Example :

How to change the user/owner associated with `file1`?

```
# chown user02 file1
```

How to change the group associated with `file1`?

```
# chown :groupA file1
```

How to change the owner and group at the same time for `file2`?

```
# chown user02:groupA file2
```

How tp change the user/group for a directory and all of its contents?

```
# chown -R user01:groupA Resources
```

The above task provides a recursive configuration. Technically, recursive commands are repeated on each specified object. Effectively, recursive means "this and everything in it." In the above example, you are configuring the related user/group for the Resources directory and everything in it. Without the -R option, you would only affect the Resources directory itself, but not its contents.

## How to manage permissions?

The _change mode_ or `chmod` command sets permissions. The syntax is straight-forward:

```
chmod permissions resource-name
```

Here are two examples of manipulating permissions for `file`:

```
# chmod 740 file
# chmod u=rwx,g=r,o-rwx file
```

The first example represents **absolute mode** and second **symbolic mode** respectively.

#### How to use absolute mode?

Absolute mode is one of two ways of specifying permissions

Each access level (read, write, execute) has an octal value:

| **Access level** | **Octal value** |
| ---------------- | --------------- |
| Read             | 4               |
| Write            | 2               |
| Execute          | 1               |

Each identity (user, group, others) has a position:

| **Identity** | **Position**       |
| ------------ | ------------------ |
| User         | First or left-most |
| Group        | Middle             |
| Other        | Last or right-most |

The absolute mode syntax states the desired permissions from left to right.

How to grant the user (owner) read, write, and execute, the group read-only, and all others no access to `file` by using absolute mode

```
# chmod 740 file
```

The three permissions values are associated with identities:  
    **ugo**  
    **740**

- The **7** is assigned to the user and is the sum of 4+2+1 or read+write+execute (full access)
- The **4** is assigned to the group and is the sum of 4+0+0 (read-only)
- The **0** is assigned to others and is the sum of 0+0+0 (no access)

In this example, the user has **rwx**, the group has **r** only, and all others have **no access** to `file`.

One more example.

How to grant the user (owner) read and write, the group read-only, and all others read-only to `file

```
# chmod 644 file
```

- The user has **6** (read and write)
- The group has **4** (read-only)
- All others have **4** (read-only)

How to set permissions for the `Resources` directory and all of its contents by using absolute mode

```
# chmod -R 744 Resources
```

#### How do I use symbolic mode?

Symbolic mode uses more symbols, but the symbols are simpler to understand.

Each access level has a symbol:

| **Access level** | **Symbol** |
| ---------------- | ---------- |
| Read             | r          |
| Write            | w          |
| Execute          | x          |

Each identity has a symbol:

| **Identity** | **Symbol** |
| ------------ | ---------- |
| User         | u          |
| Group        | g          |
| Other        | o          |

There are also operators to manipulate the permissions:

| **Task**                 | **Operator** |
| ------------------------ | ------------ |
| Grant a level of access  | +            |
| Remove a level of access | -            |
| Set a level of access    | =            |

The general `chmod` command syntax is the same:

```plaintext
command permissions directory/file
```

Here is an example:

How to remove the read permissions from others for `file` by using symbolic mode

```
# chmod o-r file
```

This example removes (`-`) the read (`r`) permission from others (`o`) for `file`.

How to set permissions for a directory and all of its contents by using symbolic mode

```
# chmod -R o=rwx,g+rw,o-rwx Resources
```

### Sticky bit

The permission system in Linux has one interesting concept, called the sticky bit. A sticky bit is a parameter that can be set on any directory. It prohibits anyone other than the owner from deleting or renaming files in it. Notice, that other users may or may not be able to edit the file. Even if they can edit it, with teh sticky bit only the owner can delete or rename the file. You can set the sticky bit on a folder with this command:

```
$ chmod +t someDirectory/
```

If the sticky bit is set, its permission string will have a `t` at the end, like this: `drwxrwxr-t`. To unset the sticky bit, use:

```
$ chmod -t someDirectory/
```
