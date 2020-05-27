# Bash Shell

## Directory Commands
- pwd : present working directory
- cd : change directory
- mkdir : make directory

## List Commands
- ls : list all files and sub directories in current directory
- ls –F : 
- ls <folder> : list files in folder
- ls –a : shows working directory folders too

## Text Commands
- tab : autocomplete 
- up key : redo previous
- touch : makes something exist
- nano : open text editor
- cat : (concatenate) ie for use with text file will output text
- less :  more advanced version of cat, prints 1 page at a time

## Remove Commands - use with care
- rm : remove
- rm –i : add a prompt before removing
- rm –i –r : remove recursive (everything inside folder also) and prompt for delete
- rm –ir : same as above

## Move / Copy Commands
- mv <file name> <new file name>: rename
- mv <file> ../<file name> : move file to parent directory
- cp <file name> ../ : copy a file

## Wildcards
- *  : wildcard (can represent 0, 1 or many chars)
- ? : wildcard (exactly 1 char)
- [AB] : wildcard, A or B

## Basic Operations
- wc : word/line/chars count
- wc –l : line count only
- wc –w : word count only
- wc – c : char count only
- ">"  : pass into file
- wc –l *.pdb > lengths.txt : take the output and put into text file
- sort : sorts data (has flags to sort dift ways)
- sort –n : sort numerically
- sort –r : reverse sort
- head –n 1 : specify number of lines to show
- tail : show bottom lines
- | : (vertical line) pipe, takes anything from left going into pipe and what comes out other end is used in next step
- echo : print whatever you’ve given 
- bash <file> : bash please run contents of <file>
- ctrl + c : stop process
- ctrl + l : move screen for prompt at top
- history : see all commands since start of session

## Loops
```
for thing in list_of_things
do
    operation_using $thing  
done

for filename in unicorn.dat basilisk.dat minotaur.dat
do
head –n 2 $filename
done

for filename in *.dat
do
head –n 2 $filename | tail –n 1
done

for filename in *.dat
do
echo $filename
head –n 2 $filename | tail –n 1
done
```
