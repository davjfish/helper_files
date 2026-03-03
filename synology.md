### to add an admin user with command line:
- add a user `sudo synouser --add myuser mypw "my user" 0 "" 1`
- add a user to admin group `sudo synogroup --member administrators user1 user2 ...`  # note must be full list!`
- get the list of admins: `sudo synogroup --get administrators` 