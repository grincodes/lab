#   Notes

# Read and use System documentation
- ls --help
- man <commands>
- sudo mandb - create a mandb that apropos can search on 
- apropos director - search for all instance of director on the man doc
- apropos -s 1,8 director - search for director in man doc section 1 and 8 which are majorly for  commands
- systemctl tab tab (for auto complete or suggestions)

# Driectories (Create,Delete and copy Directory)
 - ls -l (list files in directory in long format)
 - ls -a (list all files in directory)
 - ls -alh (list files in human readable format , must be used with l)
 - cd 
 - cd - (previous directory)
 - touch 
 - mkdir
 - cp [source] [directory]
 - cp -r [source] [directory]
 - mv [source] [directory]
 - rm 
 - stat
# Create and manage Hard links
  Inode is a data structure where all file meta data are stored except contents and name
  it contains a pointer to the actula file location on disk

  - Hard Link: creates another file that points exactly to the same inode of the orginal file
   ``` ln [path_to_the_target_file] [path_to_link_file] ```
  
  - Soft Link: creates another file that point to the file path of the original file
    ``` ln -s [path_to_the_target_file] [path_to_link_file]```

# List,Set and Change Standard Permissions
  - chgrp group_name dir/file (change group)
  - groups
  - chown user dir/file. (change userowner)
  - chown user:group dir/file (change userowner and group)
 
  - permissions
  ```
  [-(rwx)(rwx)(rwx)]  
  (-) => directory type [ - "regular file", d "directory", l "link" ..etc]
  (rwx) => user permission (r "read", w "write", x "execute" , - "no permission") 
  (rwx) => group permission (read,write, execute)
  (rwx) => other user permission (read,write, execute)
  ```

  permissions execute from left to right The first batch of the permission set is applied/used while others are ignored.
  For example 
  ```ls -l
  -r--rw--- aaron family doc.docx```
  aaron cannot update or write doc.docx because the permission is mapped to just -r-- as aaron is the owner of the file his permissions are mapped to -r-- even though he is in family group that has access
  to rw- .
  on the other hand jane in family has access to write because they are mapped to rw- because they are in the group family

  - change permissions
   
    Add to exisitng permissions
    ```
    user   u+  u+w/u+rw/u+rwx
    group  g+  g+w/g+rw/g+rwx
    other  o+  o+w/o+rw/o+rwx
    ```

    Remove from existing permissions
    ```
    user   u-  u-w/u-rw/u-rwx
    group  g-  g-w/g-rw/g-rwx
    other  o-  o-w/o-rw/o-rwx
    ```

    Set exact permissions
    ```
    user   u=  u=w/u=rw/u=rwx
    group  g=  g=w/g=rw/g=rwx
    other  o=  o=w/o=rw/o=rwx
    ```

  - combine permissions
    chmod u=r,g=rw,x= doc.docx
  
  - set all permissions
    ```
      a (all) = a+rwx  #applies to all (user,group and other)
    ```

# Suid SGid and StickyBit
  - suid allows users to execute commands with the privileges of the program owner rather than thier own privileges. it majorly used for file.
  usecase:
  and an example is /usr/bin/passwd execute permission which has suid root and allow users to run this executable with root priviledges
  cmd:
  add suid
  ```
    chmod u+s - just enables suid without execution
    chmod u+sx = add suid with executube
   ```

  - guid is allows user to use priviledges of groups instead of thier own priviledges.
    This is used majorly for directories and executables
    usecase:
    /var/mail any file or operation done on this directory is using the mail privileges
    cmd:
    ```
      chmod g+s
    ```

  - sticky bit -  allows only owner of file or director to delete a file or contents of director
    usecase:
    /tmp/ is shared by a lot of user
    cmd:
    ```
      chmod o+t
    ```
# Search files

  - find [path] [search parameters]
  - parameters
     name: search by name
      ```
        find /usr/ben/docs/ -name father.jpg
        find /usr/ben/docs/ -iname dog #case incensitive
        find /usr/ben/docs/ -name "*.jpg"
      ```

     size: search by size
      ```
        find /lib64/ +10M
      ```
     mmin: search by modified minute
      ```
      find   -mmin 5: Finds files modified exactly 5 minutes ago.
      find  -mmin +5: Finds files modified more than 5 minutes ago.
      find  -mmin -5: Finds files modified less than 5 minutes ago.
      find  -mtime 1: Finds files modified in a 24-hour window starting from 24 hours ago (i.e., between 24 and 48 hours ago). To find files modified within the last 24 hours, use -mtime 0 or -mtime -1.
      find -cmin 2: Finds files whose status (metadata like permissions or ownership) was changed exactly 2 minutes ago. Unlike mmin (modification of content), cmin refers to change in file status.

      ```

     perm: search by file perm
     ```
        find -perm 664 # find files with exactly these permissions
        find -perm -664 # find files with at least these permissions
        find -perm /664 # find files with any of these permissions
        find -perm u=rw,g=rw,o=r # find files with exactly these permissions

      ```


  - search expressions
    combining different search parameters together logically with (and , or  and not) 
    by default the and operator is applied to multi search param
    ```
    # AND operator
    find -name "feli*" -size +10M # this means both conditions have to be true for the files to show up on the result

    # OR operator
     find -name "feli*" -o -size +10M # this is an or operation which means only one expressin has to be true

    # Not operator
      find -not  -name "feli*"
      find \! -name "feli*"
    ```



