
# Writing Code in Spyder & Opening Files

*From the command line, launch spyder by typing "spyder"*

menu: FILE -> NEW FILE

````
file = open('testfile.txt','w') 
 
file.write('Hello World') 
file.write('This is our new text file') 
file.write('and this is another line.') 
file.write('Why? Because we can.') 
 
file.close() 
````

*from the command line, cat the file contents - there are no line breaks, fix with \n*

In a new file, paste in/type the function **get_file_contents**

````
def get_file_contents(filename):
    contents = []
    with open(filename, 'r') as f:
        for line in f:
            line_in_file = f.readline()
            contents.append(line_in_file)
    return contents

#main body
my_data = get_file_contents(/Users/jnugent/total.jobs.dat)
````

Note the error on the left hand of the screen:
“invalid syntax” - IDE's help guide you to see errors quickly and suggest options to fix them.
We need to put the file name in quotes like this:
````
my_data = get_file_contents(“/Users/jnugent/total_jobs.dat”)
````
We can see this worked by typing `len(my_data)` in the console, or by examining the variable window.
Functions can be kept in seperate files and reused, like a library of books.  
Open a new file in Spyder to show how this function can be imported from a different file:
  create “read_file.py” and copy the **get_file_contents** function into it, then go back to the original file.
  Erase the function (or comment it out), and type:
````
from read_file import get_file_contents
````

in console: len(my_data), 
my_data[8] -> shows it’s just a string
my_data[0].split() -> break it up into a list
data_list = my_data[0].split()
data_list[6] -> string of my value

——
algorithms
how do I solve this problem?
state the problem: get an average for each date in my dataset, put it in a dictionary

how do I get the average for each date in my dataset?  Different number of entries per day, so I can’t just take 96 entries and call that a day.

I look through each entry, find the date, compare it to the previous day’s date.  
If it’s the same date, then add the number of jobs to a variable called jobs_total.  
If it’s a different date, then divide the jobs_total by the total number of entries for that day, and put it in a dict under the mon & date.
need to track the total number of entries for any given day

——————
for entry in my_data:
    data_list = entry.split()
    day = data_list[0]
    jobs = data_list[6]
    print(date,jobs)

want to confirm this is what I want it to be, but don’t want to print all 10,000 entries, so:
for entry in my_data[:5]:
…
for my algorithm to work, I need to compare the date to the previous entries date.  If it’s the same, I add the number of jobs to a job total

add counter (include print statement)

add current_date, previous_date

——

counter = 0
current_record_day=0
previous_record_day=0

for entry in my_data[0:5]:
    data_list = entry.split()
    counter = counter + 1
    current_record_day = int(data_list[0]) #wait, what was the previous day again?
    date = int(data_list[2])
    jobs = data_list[6]
    print(date,jobs,counter)

——
now I need to count the jobs and apply logic
—

counter = 0
current_date = 0
previous_date = 0
jobs_total = 0
ave = 0
ave_list = []

for entry in my_data[0:5]:
    data_list = entry.split()
    previous_date = current_date  # yesterday
    current_date = (data_list[1] + ' ' + data_list[2]) 
    if current_date == previous_date:
        jobs = data_list[6]
        jobs_total = jobs_total + jobs  #remember to add it to itself
        counter = counter + 1
    elif current_date != previous_date:
        ave = jobs_total/(counter)
        ave_list.append(ave)
        ave = 0	#RESET for the next calculation
        jobs_total = jobs
        counter = 1

—
Give an error - division by zero.  why?  (first time through problem)

—

counter = 0
current_date = 0
previous_date = 0
jobs_total = 0
ave = 0
ave_list=[]

for entry in my_data: #made this this whole list now
    data_list = entry.split()
    previous_date = current_date  # yesterday
    current_date = (data_list[1] + ' ' + data_list[2]) 
    if (current_date == previous_date) or (counter == 0):
        jobs = int(data_list[6])
        jobs_total = jobs_total + jobs  #remember to add it to itself
        counter = counter + 1
    elif current_date != previous_date:
        ave = jobs_total/(counter)
        ave_list.append(ave)        
        ave = 0	
        jobs_total = jobs
        counter = 1

——
error - ran out of the end of the list, but it kept going.
fix:

for entry in my_data:
    data_list = entry.split()
    if data_list:   # note you can indent the block under the Edit menu
        previous_date = current_date  # yesterday
        current_date = (data_list[1] + ' ' + data_list[2]) 
        if (current_date == previous_date) or (counter == 0):
            jobs = int(data_list[6])
            jobs_total = jobs_total + jobs  #remember to add it to itself
            counter = counter + 1
        elif current_date != previous_date:
            ave = round(jobs_total/(counter))
            ave_list.append(ave)
            ave = 0	
            jobs_total = jobs
            counter = 1

——

so let’s plot this data.  Go to preferences in Spyder, select Run, then Execute In New Dedicated Console (may have to repeat this step)

At the top:
'''
import matplotlib.pyplot as plt
from read_file import get_file_contents
    
#main body
——

at the bottom:
plt.plot(ave_list)
plt.ylabel('number of jobs')
plt.xlabel('number of days')
plt.show()

———

illustrate how to do this from the command line

EXERCISE:
using this script as a starting point, plot weekly averages instead of daily averages
