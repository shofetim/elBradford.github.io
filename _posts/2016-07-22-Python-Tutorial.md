---
published: true
title: 'A Quick and Easy Python Tutorial'
permalink: /2016/quick-and-easy-python
author: bradford
layout: post
categories:
  - Technology
  - Tutorials
tags:
  - Python
---
_This is a translation from a 90 minute class I held on the basics of Python scripting._<!--more-->
_A download link for the PowerPoint .pptx file is at the bottom of the article._


![Picture1.jpg]({{site.img-dir-posts}}/Picture1.jpg)



### About Python
- Developed by Guido van Rossum
- First released in 1991
- High Level
- OOP*
- Platform Independent
- Great community
- Free and Open Source
- Easy to learn



### Python Applications
- Console / Scripts
- GUI
- Web


![Picture2.png]({{site.img-dir-posts}}/Picture2.png)



![Picture3.png]({{site.img-dir-posts}}/Picture3.png)



### Environment
- Lubuntu VM
- Python 2.7 with IDLE
 - IDLE is a basic Integrated Development Environment (IDE) for Python


![Picture4.png]({{site.img-dir-posts}}/Picture4.png)



![Picture5.png]({{site.img-dir-posts}}/Picture5.png)



### Interactive vs. Script Mode


We will start with interactive mode which gives us immediate feedback

	# python3
	>>>

Most programmers program Python in script mode. In script mode you can write, edit, load, and save your programs

	# python3 script.py



### Our First Program – “Why do witches burn?”

Open Python in interactive mode and type:

	>>> print(“Why do witches burn?”)

What happens?


Print is a _function_, everything within parenthesis are _arguments_


_Functions_ are subroutines that can be reused for common tasks, such as printing to the console



### Our First Program - Again

Let’s write our first program in _script mode_

	print(“Why do witches burn?”)
	input(“Press enter for the answer”)
	print(“Because they’re made of wood.”)

Save the file as `riddle.py`, and from another console, type:

	# python3 riddle.py



## Exercise 1

### ≈5 Minutes

Edit `riddle.py` to display the following every time it is run:

	Riddle Program
	By: [Your Name], accompanied by a(n) [adjective] [your spirit animal]
	[new line]

### Solution

	print(“Riddle Program”)
	print(“By: Bradford, accompanied by a corpulent camel spider”)
	print()



## Comments

Comments are ignored by Python, but are invaluable for programmers.


Indicated by `#`:

	# I’m a comment.
	# print(“I’m also a comment”)


## Exercise 2

### ≈3 Minutes

Edit `riddle.py` to notate the following:

	The name of your program
	The name of the developer
	The date the script was created

### Solution

	# Riddle Program
	# Bradford
	# 15 July 2015



## Documentation

https://docs.python.org/


![Picture6.png]({{site.img-dir-posts}}/Picture6.png)



## Input

Now let’s get input from the console.

	input([prompt])
	If the prompt argument is present, it is written to standard output without a trailing 			newline. The function then reads a line from input, converts it to a string (stripping a 		trailing newline), and returns that.

	https://docs.python.org/3.2/library/functions.html?highlight=input#input


	name = input(“WHAT... is your name?”)
	print(“Your name is”, name)



## Blocks

Blocks are one or more consecutive lines that form a single unit.


Other languages (C, C++, C#, Java) require curly braces `{}` to indicate the beginning and the end of a block.


Python uses _indentation_ to create blocks ---


Indentation can be either:
- A combination of spaces (most common & recommended is 4 spaces)
- A single tab


Most important rule:
**DON’T MIX INDENTATION STYLES**



## Decision Making - Conditional Statements

If statement:

	if [condition]:
		[do action]
    elif [condition]:
    	[do action]
    else:
    	[do action]

How does Python know when a conditional action ends?


If statement:

	swallow = “european"
	if swallow == “african":
    	print(“It could grip it by the husk.”)
    elif swallow == “european”:
    	print(“They could carry it on a line.”)
    else:
    	print(“Are you suggesting coconuts migrate?”)



## Exercise 3

### ≈10 Minutes

Edit `riddle.py` to perform the following:

- Prompt for the user to select a riddle by typing a 1 or 2
- Based on the input, display a riddle
- Output an error if input either than “1” or “2” is entered

### Solution

	number = input("Which riddle would you like to hear? Enter 1 or 2: ")
	if number == “1”:
		print(“Why do witches burn? Because they’re made of wood!")
	elif number == “2”:
		print(“What is your favorite color? I don’t know!")
	else:
		print(“Ni!")



## Loops

### While Loop

	while [condition]:
		[loop body]

The loop will execute the loop body until the condition is no longer true.

	limbs = 4
	print(“None shall pass.”)
	while limbs > 0:
		print(“C’mon ya pansy!”)
		print(limbs, “limbs left”)
		limbs -= 1
	print(limbs, “limbs left”)
	print(“Alright, we’ll call it a draw.”)

What does the above loop do?



## Exercise 4

### ≈10 Minutes

Edit `riddle.py` to perform the following:


Ask the user a riddle. Implement a loop that continues to loop until the user provides the right answer.

Bonus: make the input case-insensitive [string.lower()]



## Functions

Let’s create our own function, `print_riddle`, in our `riddle.py`:

	def print_riddle():
		print(“Why do witches burn?”)
		print(“Press enter for the answer”)
		print(“Because they’re made of wood.”)

	print_riddle()

How does Python know when the function begins? Ends?

## Python Resources

[This tutorial (PowerPoint Presentation)](/assets/bin/Python_for_Beginners.pptx)

[Python.org Beginner's Guide](https://wiki.python.org/moin/BeginnersGuide)

[Python.org Beginner's Guide for Programmers](https://wiki.python.org/moin/BeginnersGuide/Programmers)

[The Hitchhiker's Guide to Python](http://docs.python-guide.org/en/latest/)

[Safari Books Online (5712 results for 'Python')](http://techbus.safaribooksonline.com/search?q=python)

[Python Documentation](https://docs.python.org/)
