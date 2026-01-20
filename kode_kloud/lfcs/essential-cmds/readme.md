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

