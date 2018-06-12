# Day 1 Practice Session Exercises

The exercises below should be completed using the interactive Python
interpreter. You are expected to have installed the Anaconda Python distribution
(version 3.6).

On Windows, click Start -> Anaconda3 -> Anaconda Prompt, then type `python` at
the prompt to start the interpreter.

On Mac, open a Terminal window and type `python` at the prompt.

## Exercises

### Math: Integers and floats

When is there a difference between using an integer value (no decimal) and the
decimal equivalent of a number? For example 3 and 3.0. Play around with some
common math operations to test it out.

### Math: Floating point weirdness

Add `0.1 + 0.2`. What's going on?

See [http://introtopython.org/var_string_num.html#Floating-Point-numbers](http://introtopython.org/var_string_num.html#Floating-Point-numbers)
for more information.

### Variables

If you run the code below, what will the output be? Figure it out before running it.

```python
>>> a = 1
>>> b = a + 2
>>> a = 2
>>> print(b)
```

### Variables: Assigning multiple values at once

If you run the code below, what will the output be? Figure it out before running it.

```python
>>> first, second = 'Grace', 'Hopper'
>>> third, fourth = second, first
>>> print(third, fourth)
```
If you don't know who Grace Hopper is, [take a look](https://en.wikipedia.org/wiki/Grace_Hopper).

### String concatenation

Store your first name in a variable. Store your last name in a different
variable. Combine (concatenate) them to make a single string.

### String slices

You can index and slice strings. Print the first and last letters of your
first name variable from above.

### Creating lists

Make a list called `words` that has 10 words of your choice in it.

### List indexing and slicing

Print the first and last elements of the list. Then the first two elements as a
subset of the list.

### Modifying lists

Add two words to the end of your list, then print it out.

Challenge: Add two words to the *beginning* of your list, then print it out.

Now remove the words you added to your list, and print it out again.

### List slicing, continued

Print every other word in your `words` list starting with the second one.

Challenge: Print your list in reverse order without changing the order of the list permanently.

### Nested lists

You can have nested lists. Create a list that has two other lists as its
elements. Save it in a variable called `nested_list`.

Print the first list from `nested_list`. Then print the second element of the
first list.

### Word Problem!!

#### Part 1

You write software for Porta-Party, a company that coordinates car pools for
ride sharing. You have been asked to calculate how many unassigned seats there
are in the car pool, so the Porta-Party app knows how many people can still sign
up for rides.

- There are 14 cars in the car pool.
- Each car has 4 seats.
- One seat in each car is given to a driver, and is not available for ride-sharing.
- 28 users have signed-up for rides in the Porta-Party car pool.

Calculate the available seats for ride-sharing customers in the Porta-Party
using variables you've assigned, such as `available_cars`, `seats_per_car`,
`drivers_per_car`, etc.

How many users can ride in the car pool? With 28 users signed up, how many more
users can sign-up for a ride?

Print the results. Include an if/else statement to print out either "Yes, you
can sign up for the car pool!" or "No room in the porta-party!" depending on how
many users have signed-up for a ride.

Change the number of users signed up so you can see each possibility printed.

#### Part 2

As an employee of Porta-Party, you are required to drive one of the car-pool
cars. Three users have signed up to be in your car-pool car.

Create a list with their three names (pick any three names, like Janna). Using
the list to access the names, print out a welcome message to each person,
capitalizing their name ("Welcome to the car pool, JANNA")

Use the `len()` function on the list to find out how many items are in the list.
Now use the `len()` function on each of the names on the list to figure out how
many letters each name contains.

Using `len()`, calculate the average name length of the members of your car
pool.

### Fixing Syntax Errors

Fix the errors in the code below.

```
2010data = [3,5,7,9,11]
print("The first 2010 data value is "+2010data[0])

mycolors = ["red","green","blue",["purple","pink","teal"]]
# make the sentence below make sense with values from mycolors above
 print(mycolors[1]+" and "mycolors[3]+" mix to make "+mycolors[4]) 

my_list=("apples","bananas","pears)
print(mylist)
my_list[0] = "avocado"
print(my_list)
```