
# Writing Code in Spyder & Opening Files

From the command line, launch spyder by typing "spyder".

## Writing a file

menu: FILE -> NEW FILE

````
file = open('testfile.txt','w') 
 
file.write('Hello World') 
file.write('This is our new text file') 
file.write('and this is another line.') 
file.write('Why? Because we can.') 
 
file.close() 
````

Confirm this file exists: from the command line, cat the file contents.  Note there are no line breaks, fix by adding the newline character \n to the end of each line.

## Reading from a file

In a new file called **data_analysis.py** (menu: FILE -> NEW FILE), paste in/type the following function **get_file_contents**:

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
To fix that error we need to put the file name in quotes like this:
````
my_data = get_file_contents(“/Users/jnugent/total_jobs.dat”)
````
Now the code runs without errors.  Confirm this worked by typing `len(my_data)` in the console, or by examining the variable window.  my_data should now have many lines in it.

## Create your own library 
Useful, generic functions can be kept in their own files and reused from program to program. Experienced programmers may accumulate a collection of functions they have written and like to have on hand, like books in a library.  
To show how this function can be imported from a different file:
Open a new file called **read_file.py** in Spyder (menu: FILE -> NEW FILE) 
Copy the **get_file_contents** function into it, then go back to the file you named **data_analysis.py**.
In **data_analysis.py**, comment out **get_file_contents**, and type:
````
from read_file import get_file_contents
````
Run the code in **data_analysis.py**.  It works as if the **get_file_contents** function was still there, because it's importing it from **read_file.py**.  

### Extracting Data

In the Spyder console type len(my_data), and look in the variables inspector window to see what you can learn about my_data.
If you access a single line of the data, you can see that it's just a string: to confirm this, in the console window, type ```my_data[8]```
We need to break this up to make it useful: type ```my_data[8].split()``` to break it up into a list.
To save this into a list, type ```data_list = my_data[8].split()```; to access the jobs number type ```data_list[6]```.  Note that this is a string of the number.


### Algorithms
How do I write a program to analyze this data?  
The first step is to **state the question** I'm trying to answer: Is the number of jobs trending upward, downward or staying flat?

**What I want the output of my program to be**: a plot of jobs over time so I can see the trends.

Why this is tricky: I have too many variable data points to plot directly and make any sense out of it.  I want to plot the average value of jobs for each day, but my dataset has an inconsistant number of job number samples for each day.  The number of jobs is recorded every 15 minutes, except sometimes it isn’t - how to get average job numbers per day when the sample size is different every day?

**State the problem**: how do I get the average for each date in my dataset?  There are a different number of entries per day, so I can’t just take 96 entries and call that a day.

**State the solution**:  Look through each entry, find the date, compare it to the previous day’s date.  
If it’s the same day, then add the number of jobs to a variable called jobs_total and read the next entry. 
If it’s a different day, then divide the jobs_total by the total number of entries for that day, and put it in a list.
To calculate the average, I need to track the total number of entries for any given day.

**Write it in pseudocode**: 
Open the data file 
for each line of data:
    get the current day    
    get the number of jobs 
    if the current day is still the day from the last record:
        add the # of jobs to the total # of jobs for the day
        increment the number of job samples for the day
    elif the current day has changed from the last record:
        calculate the average number of jobs for the day
        put the ave for the day into a list
        reset the variables for the next iteration 

### Building The Code
In the file **data_analysis.py**, add this in main after the file has been read:
````
for entry in my_data[:5]:
    data_list = entry.split()
    day = data_list[0]
    jobs = data_list[6]
    print(date,jobs)
````
I want to confirm this is doing what I want it to without printing all 10,000 entries, so I'm only looping through a slice of the list for now.

For my algorithm to work, I need to compare the date to the previous entries date.  If it’s the same, I add the number of jobs to a job total.  I need variables to keep track of this - counter, current_record_day, and previous_record_day:
````
counter = 0            #add this
current_record_day=0   #add this
previous_record_day=0  #add this

for entry in my_data[0:5]:
    data_list = entry.split()
    counter = counter + 1
    current_record_day = int(data_list[0])     #wait, what was the previous day again?  
    day = int(data_list[0])
    jobs = data_list[6]
    print(date,jobs,counter)
````
Previous day has to be filled with current day's value before writing over current day.
Now I need to count the jobs and apply logic:
````
counter = 0
current_date = 0
previous_date = 0
jobs_total = 0       #add this
ave = 0              #add this
ave_list = []        #add this

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
````

Run this code.  It gives an error - check the stack trace and you'll see it's complaining about "division by zero".  Why?

````
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
    if (current_date == previous_date) or (counter == 0):       # the fix for the first time problem
        jobs = int(data_list[6])
        jobs_total = jobs_total + jobs  #remember to add it to itself
        counter = counter + 1
    elif current_date != previous_date:
        ave = jobs_total/(counter)
        ave_list.append(ave)        
        ave = 0	
        jobs_total = jobs
        counter = 1
````
Now when we run the code we have another error - ran out of the end of the list, but it kept going.  This can happen if there are empty lines at the end of the file where we are trying to extract data.  How can we fix this?

````
for entry in my_data:
    data_list = entry.split()
    if data_list:                           # note you can indent the block under the Edit menu
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
````
The code is running cleanly now, so let’s plot the ave data list to look for trendlines.  To enable Spyder to display a plot, go to preferences in Spyder, select **Run**, then **Execute In New Dedicated Console** (may have to repeat this step at points if Spyder stops displaying plots.)

At the top of **data_analysis.py**, we need to import a library to help us create the plot:
````
import matplotlib.pyplot as plt
from read_file import get_file_contents
````
At the very bottom of **data_analysis.py** add:
````
plt.plot(ave_list)
plt.ylabel('number of jobs')
plt.xlabel('number of days')
plt.show()
````
Run this code.  

You can run your program from the command line as well - Spyder has written your code into a stand-alone file that doesn't require a GUI to run it.  Just type `python <path to data_analysis.py>` at the command line.

## EXERCISE
This usage trendline isn't very clear.  Using this script as a starting point, plot weekly averages instead of daily averages to see if that brings more clarity.
