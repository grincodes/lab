# Sed (stream editor)
Useful for string manipulation on line-oriented text file
mostly used as a filter in shell scripts

command - sed [pattern] [filepath]

command patterns

substitution (s)
sed 's/old/new/' file 
- the pattern is case sensitive and it only matches case occurence on each line
sed 's/old/new/' content.txt > output.txt 
we can use single and double quotes for pattern
we can use different delimeter to seperate old and new

sed "s/old/new/g" content.txt  for all occurrence on a line

sed "s/old/new/p" content.txt to print all occurence

By default <sed "s/old/new/g" context.txt>  prints out all the content of the file even if there
is no match or substitution.

sed "s/old/new/p" this prints duplicate (one print for default sed printing and the other for /p)

to ignore sed default printing use -n

sed -n "s/old/new/gp" content.txt

-e allow specifying multiple command 
sed -e s/old/new/p" -e s/old/new/p" context.txt




