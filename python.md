# Python CHEATSHEET

## Date and Time

- subtract time from a datetime object:
```
import datetime
t = datetime.datetime(2018,10,10,12,12,0)
new_t = t.replace(hour=t.hour+1)
```
- format a datetime object (click [HERE](https://docs.python.org/2/library/datetime.html#strftime-and-strptime-behavior) for the format string reference):
```
import datetime
t = datetime.datetime(2018,10,10,12,12,0)
t.strftime('%Y-%m-%d')
```
- get a datetime obj from a string:
```
datetime_object = datetime.strptime('Jun 1 2005  1:33PM', '%b %d %Y %I:%M%p')
```



## loop through files in filesystem
```
import os

for subdir, dirs, files in os.walk(rootdir):
    for file in files:
        #print os.path.join(subdir, file)
        filepath = subdir + os.sep + file

```

## loop through csv file
```
with open('eggs.csv', 'rb') as csvfile:
     spamreader = csv.reader(csvfile, delimiter=' ', quotechar='|')
     for row in spamreader:
         print(', '.join(row))
```


## double for loop / Double Iteration in List Comprehension
from: https://stackoverflow.com/questions/1198777/double-iteration-in-list-comprehension

```
# double for loop
list_of_words = []
for sentence in text:
    for word in sentence:
       list_of_words.append(word)

# list comprehension with two iterations
[word for sentence in text for word in sentence]

[target_item for big_loop_iterable_item in big_loop_collection for target_item in big_loop_iterable_item]

```

```
# double for loop
list_of_people = []
for org in org_list:
    for person in org.person_list:
       list_of_people.append(person)

# list comprehension with two iterations
[person for org.person_list in org_list for person in person_list]
```

## pip Commands
```
#list installed packages
pip freeze

#check pip version
pip --version

#upgrade pip
pip install -U pip

#verify installed packages have compatible dependencies
pip check

#You can also put a list of packages (one per line) in a text file and install all of them at once using the 
#-r or --requirement flag.
pip install -r requirements.txt

#Other commands 
https://pip.pypa.io/en/stable/reference/
```

