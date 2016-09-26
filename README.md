#!/usr/bin/bash

### dirtysurf.sh ###

### Simple bash script to take in multiple text files, ###
### search the contents for all instances of 'username' ###
### and 'password' and organize the results into a single file. ###
### Run textsurfer.sh from same directory as the files you want surfed. ###

### Creates list of unsorted usernames. List includes garbage. ###
/usr/bin/grep -B0 username ./* --exclude dirtysurf.sh > user-dirty.txt

### Removes garbage from username list as well as old list file. ###
sed 's/^.*:/username:/' user-dirty.txt > user-clean.txt
rm user-dirty.txt

### Repeats previous process to create list of unsorted passwords. ###
/usr/bin/grep -B0 password ./* --exclude dirtysurf.sh user-clean.txt > pass-dirty.txt
sed 's/^.*:/password:/' pass-dirty.txt > pass-clean.txt
rm pass-dirty.txt

### Removes duplicates from lists and numbers them beginning at 1. ###
###cat user-clean.txt | uniq -u > user-temp.txt && cat -n user-temp.txt > userlist.txt
###cat pass-clean.txt | uniq -u > pass-temp.txt && cat -n pass-temp.txt > passlist.txt

### changed previous step to include all lines including repeats ###
cat -n user-clean.txt > userlist.txt
cat -n pass-clean.txt > passlist.txt

### Removes garbage ###
rm user-clean.txt pass-clean.txt ###user-temp.txt pass-temp.txt###

### Create directory for newly created lists and dumps lists inside ###
mkdir Lists
mv userlist.txt passlist.txt Lists

### changes directory to match newly created files ###
cd Lists

### create master file ###
paste -d '\n' userlist.txt passlist.txt > masterlist.txt
