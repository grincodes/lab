#   Notes

# Read and use System documentation
- ls --help
- man <commands>
- sudo mandb - create a mandb that apropos can search on 
- apropos director - search for all instance of director on the man doc
- apropos -s 1,8 director - search for director in man doc section 1 and 8 which are majorly for  commands
- systemctl tab tab (for auto complete or suggestions)

# Directories (Create,Delete and copy Directory)
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

# Compare and manipulate text file

```
sed [pattern] [path]
```
flags
-n : ignore sed default printing
-e : for mutiple sed commands
-i : execute or edit inplace
patterns 
/g: substitute all occurences
/p: prints all occurences

```
sed "s/old/new/g"
```

Cut: extract text from file using a delimeter
```
cut -d ' ' -f 1 userinfo.txt
```
The  -f is called field , the text are broken into delimeters and seperated by fields

Unique: remove repeating lines that are next to each other

```
 uniq context.txt
```
Sort : sort lines of text alphabetically 
```
 sort content.txt
```
Diff: check differnce between files

```
diff file file2
```
```
diff -y file file2
```

# Pager and Vim text editor

less content.txt
more content.txt
Vim commands
h: cursor left
j: cursor down
k: cursor up
l: cursor right
^: first character on the line
$: last character on the line
Ctr o + ^ : first character of the line in insert mode.
This temporarily switches to normal mode for one command ($) 
Ctr o + $ : last character of the line in insert mode.
This temporarily switches to normal mode for one command ($) 

# Search for text with Grep

Grep [pattern] [filepath]
flags
-i : ignorecase
--color: color match
-wi: match word
-vi: return all text that doesn't match pattern
-r: recursively search all files in a folder for match
-ri: group recursive and ignorecase flag 
-oi: returns only matching pattern/text


# Analyze text using regular expression
regex expressions
- ^: line begins with
- $: line ends with
- .: match any one character
- *: match previous elements and more items after
- +: match previous element 1 or more items 
- {min num of repetiton, maximum num of repetition}: previous element can exist "this many times"
- ?: make the previous element optional
- |: match one thing or the other thing
- []: ranges or set [a-z][akol]
- ():sub expressions
- [^]: negate ranges or set.


# Archive,Backup,Compress and Uncompress

  Archive:
  Tar (Tape archive) Utility

 - List files in an archive
  ``` tar --list --file archive.tar || tar -tf archive.tar || tar tf archive.tar ```

 - Create archive
  ``` tar --create --file archive.tar file1/directory || tar cf archive.tar file1/directory  ```

 - Append file to archive
  ``` tar --append --file archive.tar file2 || tar rf archive.tar file 2```

 - Extract archive
  ``` tar --extract --file archive.tar || tar xf archive.tar```
 - Extract archive to another directory
   ``` tar --extract --file archive.tar --directory /tmp/ || tar xf archive.tar -C /tmp/```
 - Archive and Compress
  ``` tar --create gzip --file archive.tar.gz file1 || tar czf archive.tar.gz file1  ```
 - Archive with auto compression
  ``` tar --create --autocompress --file archive.tar.gz file1 || tar caf archive.tar.gz``` 

  
# Compress and Decompress
   gzip,bzip,xz
  
    gzip Utility
  - Compress
   ```gzip  file -> file.gzip```

      keep original file after zip
   ```gzip --keep  file -> file.gzip```
    
  - Decompress 
    ```gunzip file.gzip```

    zip Utility
    ```  zip archive file ```
    ``` zip -r archive.zip Pictures/ ```

    ``` unzip archive.zip ``` 

# Backup Files to Remote
   resync

  - Local to Remote
   ``` resync -a host/directory  user@host:/remote/direcory ```
  
  - Remote to Local
    ``` resync -a user@host:/remote/direcory  host/directory ```

# Disk Imaging
  dd

   ``` sudo dd if=/dev/vda of=diskimage.raw bs=1M status=progress ```
    if= inputfile
    of= output file
    bs= block size

# Use Input-Output Redirection (e.g. >, >>, |, 2>)
   > - rewrite to file
   >> - append to file
   1> - standard output
   2> - standard error
   2>&1 - standard error to (&1) - standard out


  Redirecting input 
   ``` sendemail mail@gmail.com  < input.txt ```
   Heredoc 
    ``` 
    sort << EOF
    < 2
    < 4
    < 1
    < EOF
    ```

  Here string
  ```bc <<<1+2+3```

# Work with SSL Certificates
  ssl vs tls
  Tls is a more recent and secured version of ssl used to encrypt http requests and authenticate website

  openssl command supports many cryptographic operations. 
  The cryptographic operation for ssl is x.509 certs.
  cert signing request (CSR)

  Generate key and CSR 
  ```openssl req -newkey rsa:2048 -keyout key.pem -out req.pem```

  Generate self signing tls cert
  ```openssl req -x509 -noenc -newkey rsa:2048 -days 365 -keyout privatekey.pem -out mycrt.crt```

  Verify and details of a cert
  ```openssl x509 -in mycrt.crt -text```

# Git basic operations | Git staging and commiting changes | Git branches and remote repositories
 ```
  git config --global user.name "name"   - configure git global user
  git config --global user.email "email" -  configure git global user
  git init - initialize git project
  git status - check current git status 
  git reset file - undoing changes and reseting head  
  git rm file - remove file from staging area
  git add . - add files to staging area
  git commit -m "" - commit files changes to local repo 

  git branch nam  - create a branch
  git branch --delete name - delete a branch
  git checkout branch - switch to a branch
  git commit -a -m "commit message"  - add and commit changes
  git log - check git logs
  git log --raw. - check git  raw logs
  git show hash - show changes on hash
  git merge branch - merge branch
  git remote -v - check git remote url
  git remote add origin url - add remote url to repo
  git push origin master - git push local to remot 
  git pull origin master - git pull from remote to local
  git clone repo -  clone remote repo to local
  ```

# Boot,Reboot, and Shutdown a System
  ```
    systemctl reboot
    sytemctl poweroff 

    systemctl reboot --force
    sytemctl poweroff --force
    systemctl reboot --force--force

    #scheduled reboot or shutdown
    sudo shutdown 02:00 <24hrs time>
    sudo shutdown +15 (shutdown in 15 mins)
    #reboot -r
    sudo shutdown -r 02:00 <24hrs time>
    sudo shutdown -r +15 (shutdown in 15 mins)

    # add wall message for users
       sudo shutdown -r +15 'Scheduled restart to upgrade our linux kernel' 

  ```

  # Boot or  change system into different modes
  ```
   # to get boot target
    systemctl get-default

    systemctl set-default multi-user.target
    systemctl isolate graphical.target
    
    #other targets 
    - emergency.target
    - rescue.target'
  ```

  # Using scripting to automate system maintenance tasks
    ```
      #!/bin/bash
      
      date >> /tmp/script.log
      cat /proc/version >> /tmp/dcript.log
    ```

    ```
      #!/bin/bash

      if test -f /tmp/archive.tar.gz ;then
          mv  /tmp/archive.tar.gz /tmp/archive.tar.gz.OLD
          tar acf /tmp/archive.tar.gz /tmp/apt

      else
         tar acf /tmp/archive.tar.gz /tmp/apt
      
      fi

    ```

    ```
      #!/bin/bash

      if grep -q '5' /etc/default/grub; then
        echo 'Grub has timeout of 5 seconds.'
      else
        echo 'Grub DOES NOT have a timeout of 5 seconds.'
      fi
    ```
    ```
      more scripts examples on /etc/cron.monthly/0anacron
    ```
# Manage startup process and services

  - intitalization system (systemd) - system initialization and monitoring
  - systemd  units
     - service,socket,device,timer
  - systemd services cmds
    - sudo systemctl edit --full ssh.service
    - sudo systemctl revert ssh.service
    - sudo systemctl stop ssh.service
    - sudo systemctl start ssh.service
    - sudo systemctl restart ssh.service
    - sudo systemctl enable ssh.service
    - sudo systemctl enable --now ssh.service
    - sudo sytemctl is-enabled <service>
    - man systemd.service

# Create systemd services
  ```
    #!/bin/bash
    echo  "My App Started" | systemd-cat -t MyApp -p info # -p is log priority
    sleep 5
    echo  "My App Crashed" | systemd-cat -t MyApp -p err 

  ```
  app.service
  ```
    [Unit]
     Description=My Application
     After=network.target auditd.service

    [Service]
    ExecStartPre=echo "systemd is prepating to start MyApp"
    ExecStart=/usr/local/bin/myapp.sh
    KillMode=service #control-group is another option
    Restart=always 
    RestartSec=1
    Type=simple #notify | oneshot

    [Install]
    WantedBy=multi-user.target
  ```

  when service files are add or updated we need to reload the systemd with this command
  ```
    sudo systemctl daemon-reload
  ```
  ``` sudo journalctl -f ``` system logs on follow mode

# Diagnose and manage process
 - ps -a | ps a
 - ps aux
 - top 
 - ps <PID>
 - ps u <PID>
 - ps u -U <username>
 - pgrep -a syslog
  **N/B**
   ```
    Nice is a command used run a program with a modified scheduling priority
   ```
 - nice -n <nicevalue> <command>
 - ps l
 - ps lax
 - ps fax
 - ps faux
 - renice <oldnicevalue> <newniceness>
 **N/B**
   ```Kill is a linux command that sends signals to a process
    It does not always "kill" a process.
    Process signals are signals sent to a process to notify them about a (user/system) event 
   ```
 - kill -L
 - sudo kill -SIGHUP <PID> (SIGHUP is a)
 -  background , foregrounds and pause
    **Background**
    To run a command in the background add the & after it
    ``` sleep 180 & ```
    To check background jobs use ```jobs``` command

    ```bg 1 ``` resume background job
    **Pause**
    To pause a program use ``` Ctrl + v ```
    ***Foreground**
    TO bring program to foreground use ``` fg 1```

    ***N/B**
    ``` A program is a passive, static set of instructions stored on a disk (e.g., an .exe file), while a process is an active, dynamic instance of that program currently executing in memory. ```

    List files or directories a process is using
    ```lsof -p <pid> 
        -u	List files opened by a specific user.	lsof -u username
        -p	List files opened by a specific process ID (PID).	lsof -p 1234
        -c	List files opened by a specific command/process name.	lsof -c ssh
        -i	Display network-related information (sockets and ports).	lsof -i
        +D	Recursively list all files opened under a specific directory.	lsof +D /var/log
    ```

  # Locate and Analyse system log files in linux
   - rsyslog (rocket fast system for log processing) - it stores all logs in  /var/log/ folder
      search logs through normal serach utility commands
        ```
          grep -r 'ssh' /var/log/
          less /var/log/auth.log
        ```
  - folowing logs with 
      ```
        tail -f /var/log/auth.log
      ```
  - journalctl
      ```
          # Filter logs generated from a specific command
            which sudo -> gets command bin path
            journalctl /usr/bin/sudo

          # get logs from service unit
            journalctl -u ssh.service

          # get all logs
            journactl

          # follow mode 
            journalctl -f

          # filter logs bash on error priority
            journalctl -p err|info|warning|crit
            journalctl -p err -g '^b' #with grep option
          
          # get logs based on time
            journalctl -S 02:00 # time is based on 24hrs
            journalctl -S 02:00 -U 02:00 # time based on range
            journalctl -S '2025-03-03 01:00:30' # based on datetime string
          
          # get logs since last boot
            journalctl -b 0
            journalctl -b -1
             
      ```

  - last & lastlog
       ``` last #command to get user sessions 
            last log # command to get when users last logged in
       ```

  # Schedule Tasks to Run at a Set Date and Time
    cron, anacron, at -> utilities to schedule jobs

    cat /etc/crontab #example of cron jobs

    cron expression has 5 options
    Minutes,Hour,Day of month, Month, Day of week (Misses Job when switched off)

    Anacron
    Day, Few Days,Week,Month, Year (Doesnt miss job)

    At - For task that should only run once

    Cron 
    * - matches all possible value
    , - multiple values
    - -> to define range of values
    / - specifies steps

    crontab -e
    35 6 * * * /usr/bin/touch test_passes 
    ***N/B cron uses full command path like /usr/bin/touch***
    www.crontab.guru to practice cron

    crontab -l # list cron table
    sudo crontab -e -u jane # edit crontab of user


    special directory to run jobs
    /etc/cron.daily
     /etc/cron.weekly
      /etc/cron.hourly
       /etc/cron.mpnthly

      ***N/B dont use any extension for sheel script used in cron ***

      sudo vim /etc/anacron

      at '15:00'
      atq -c
 
  # Managing software with package manager  

    - apt update
    - apt upgrade
    - apt install
    - dpkg --listfiles nginx
    - apt show 
    - apt search --names-onyl nginx
    - apt remove
    - apt autoremove nginx remove module with all dependencies

  # Configure the Repositories of Package Manager
    - vim /etc/apt/sources.list # local storage of list of repositories
    - gpg --dearmor key

  # Install Software by compiling source code
    - sudo make
    - sudo mmake install
  
  # Verify Integrity and Availability of Resources and Processes
    - df -h # disk filesystem
    - du -sh /usr/ # disk usage to check disk usage of files and directory
    -  free -h 
    - uptime
    - lscpu
    - lscpi
    - xfs_repair -v /dev/vdb1
    - sudo fsck.ext4 -v -f -p /dev/vdb1
    - systemctl --list-dependencies

  # Change Kernel Runtime Parameters, Persistent and Non-Persistent
    - sysctl -a
    - ls /etc/sysctl.d #where configs are listed
    - sudo sysctl -p /etc/sysctl.d/swap-less.conf # apply changes immediately

  # List and identify SELinux file
    - ls -z
    - ps -z
    - getenforce
    - ps auxZ | grep sshd > /home/bob/sshd
  
  # Create and Enforce MAC Using SELinux
    apparmor.service
    selinux.basics auditd
    sestatus
    selinux-active
    grub #bootloader
    audit2why --all | less
    ps -ez | grep ssh_t
    sentenforce 1
    getenforce
    sudo audit2allow --all -M mymodule
    semodule -i mymodule.pp
    sysctl -w net.ipv6.conf.lo.seg6_enabled=1
    chcon -t httpd_sys_content_t /var/index.html (change selinux context)
  
  # Create and manage containers
    - docker search <image>
    - docker pull image
    - docker images
    - docker rmi <image>
    - docker run <image>
    - docker run --detach --publish 8080:80 --name webserver nginx
    - docker ps -a
    - docker start containerId
    - docker stop containerid
    - docker rm <container>
    - docker run --detach --publish 8080:80 --name webserver --restart always nginx
    - docker build -t custom-nginx:1.0 custom-nginx/docker

  # Manage and Configure Virtual Machines
    QEMU - Quick Emulator
    KVM -  Kernel based virtual machine
    VIRSH - command line tool for managing virtual machines
    - sudo apt install virt-manager
    - sudo vim testmachine.xml
        ```
            <domain type="qemu">
                <name> TestMachine </name>
                <memory unit="Gib" > 1</memory>
                <vcpu>1<vcpu>

                <os>
                    <type arch="x86_64">hvm</type>
                </os>
            </domain>
        ```
      - virsh define testmachine.xml
      - virsh list | virsh list --all
      - virsh start TestMachine
      - virsh reboot TestMachine
      - virsh reset TestMachine
      - virsh shutdown TestMachine
      - virsh destroy TestMachine
      - virsh undefine TestMachine | virsh undefine --remove-storage TestMachine
      - virsh autostart TestMachine
      - virsh autostart --disable TestMachine
      - virsh setvcpus TestMachine 2 --config| virsh setvcpus TestMachine 2 --config --maximum
      - virsh setmem

    # Create and Boot Virtual Machines
      create and boot virtual machine with cloud images
    

    # Installing an Operating System on a Virtual Machine
      create and boot virtual machine from disk or external drive
      wget <cloud-image-url>
      qemu-img info image.img
      qemu-img resize image.img 10G
      ls /var/lib/libvirt
      virt-install --osinfo <os-name> --name ubuntu1 --memory 3072 --vcpus 1 --import --disk /var/libvirt/mages/ubuntu-24.04-install.img --graphic none
      ctr + ] to exit from virtual machine
      sudo apt install libguestfs-tools "to set login on virtual machine"
      sudo virt-customize -a <image-path> --root-password password:password124
      virsh start




