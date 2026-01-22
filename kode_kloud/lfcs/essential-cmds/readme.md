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
