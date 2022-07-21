# Data Sharing

Users should use File Access Control Lists (FACL) to share their data and collaborate with other users. FACL mechanism allows a fine-graiNet control access to any files by any users or groups of users. FACL allows to grant access without modifying file ownership and without changing POSIX permissions.

```{admonition} Warning
:class: error

Users are discouraged from setting '777' permissions with chmod, because this can lead to data loss (by a malicious user or unintentionally, by accident).
```

ACL mechanism, just like regular Linux access controls (POSIX), allows three different levels of access control:

- Read (r)
- Write (w)
- Execute (x)

This level of access can be granted to 

- user (owner of the file)
- group (owner group)
- other (everyone else)

## View Permissions

Use `getfacl` to retrieve access permissions for a file. 

```bash
getfacl myfile.txt

# file: myfile.txt
# owner: ab123
# group: users
```
Expected output:
```bash
user::rw-
group::---
other::---
```

The example above illustrates that in most cases ACL looks just like the chmod-based permissions: owner of the file has read and write permission, members of the group and everyone else have no permissions at all.

````{tip}
You can see with 'ls -l' if a file has extended permissions set with setfacl: the '+' in the last column of the permissions field indicates that this file has detailed access permissions via ACLs:

```bash
ls -la
```

Example Output:

```bash
total 304
drwxr-x---+   18 ab123 users  4096 Apr  3 14:32 .
drwxr-xr-x  1361 root  root      0 Apr  3 09:35 ..
-rw-------     1 ab123 users  4502 Mar 28 22:27 my_private_file
-rw-r-xr--+    1 ab123 users    29 Feb 11 23:18 dummy.txt
```
````

## Modify Permissions

Permisions can be modified by `setfacl` commmand.

```bash
# General syntax:
setfacl -[options] [action/specification] <file/dir>
```
Options:
- `-m` - modify
- `-x` - remove
- `-R` - recursive (apply ACL to all content inside a directory)
- `-d` - default (set given settings as default - useful for a directory - all the new content inside in the future will have given ACL)
- `-b` - Remove all extended ACL permissions.

Specifications:
- `u:NetID:permissions` for sharing with a user.
- `g:GroupName:permissions` for sharing with a group.
- `o:NetID:permissions` for sharing with everyone.

Permissions:
- `---`, `r` for read, `w` for write, `x` for execute, for example to give only read write permissions use `rw-`.


### Examples
<br>

#### Share a file
```bash
# setfacl -m "u:NetID:permissions" <file>
setfacl -m "u:jh12524:rwx" abc.txt
```

#### Share a directory and all its files
For example folder is at `/scratch/ab1234/abc/def` then:
```bash
# setfacl -m "u:NetID:permissions" <dir>
setfacl -m "u:jh12524:r-x" /scratch/ab1234
setfacl -m "u:jh12524:r-x" /scratch/ab1234/abc
setfacl -Rm "u:jh12524:rwx" /scratch/ab1234/abc/def
```

```{important}
Give Access to Parent Directories in the Path. When you would like to set ACL to say /a/b/c/example.out,  you also need to set appropriate ACLs to all the parent directories in the path. If you want to give read/write/execute permissions for the file /a/b/c/example.out, you would also need to give at least r-x permissions to the directories: /a,  /a/b, and /a/b/c.
```

#### Share all new files in a directory
```bash
# setfacl -d "u:NetID:permissions" <dir>
setfacl -d "u:jh12524:rwx" abc
```

####  Remove a specific permission entry
```bash
setfacl -x "entry" <file/dir>
```

#### Remove all permissions
```bash
# setfacl -b <file/dir>
setfacl -b abc.txt
```
