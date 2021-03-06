```python
# -*- coding: utf-8 -*-
"""
model.py 8.0

Copyright (c) School of Geography.
University of Leeds, Leeds, West Yorkshire, UK, LS2 9JT.

Created on Sat Oct 28 11:28:48 2017

@author: Ourania Sermpezi
"""

#model.py

import random
import matplotlib.pyplot
import matplotlib.animation
import agentframework
import csv


# Creating a variable for the number of agents.
    
num_of_agents = 10


# Creating a variable for the number of iterations for the random loop.

num_of_iterations = 100


# Creating a new variable to set up the neighbourhood of all agents.

neighbourhood = 20


# Creating an empty list for the CSV reader to store the data in.

environment = []


# Creating an empty list.

agents = []


# Creating a top level container for all plot elements.

fig = matplotlib.pyplot.figure(figsize=(7, 7))


# Adding an axes at position left = 0, bottom = 0, width = 1 and height = 1,
# relevant to the size of the figure type object called 'fig'.

ax = fig.add_axes([0, 0, 1, 1])


# Reading the txt file through the csv reader and saving the data into a 2D
# list called 'environment'.

with open('in.txt', newline = '') as f:
    # Turning all numbers into floats.
	
    reader = csv.reader(f, quoting = csv.QUOTE_NONNUMERIC)
    for row in reader:
        # Setting up another local list.
		
        rowlist = []
        # For each value in the row, append it to rowlist. Once the row is
        # full, move onto the next row until all of them are full of values
        # which are appended to rowlist.
		
        for value in row:
            rowlist.append(value)
        # Once rowlist is full of values, append rowlist in environment, so
        # that each row is a new list in environment.
		
        environment.append(rowlist)
    

# Filling the 'agents' list with the 'environment' type list containing all the
# data from the txt file. Also adding a link to the list of agents into each agent.

for i in range(num_of_agents):
    agents.append (agentframework.Agent(environment, agents))


# Setting up a variable for continuing the animation.

carry_on = True


# Setting up an Agent type object.

stored = agentframework.Agent(environment, agents)


def update(frame_number):
	"""
	Move and feed the agents, while sharing their information with each other.
	Contribute to the mapping of the agents and their environment in an
	animated manner.
	
	Argument:
	frame_number -- number of frame.
	"""
    
    # Clearing the figure for the animation.
    
    fig.clear()
    
    
    # Confirming that the variable to be used later on is the same as the
    # global one.
    
    global carry_on
    
    
    # Creating a loop to move the coordinates num_of_iterations times, allow them
    # to eat from the environment and calculate the distance between each other to 
    # check whether they are within their neighbourhood and potentially alter their
    # values.
    
    for j in range(num_of_iterations):
        for i in range(num_of_agents):
            # Randomising the order in which agents are processed each iteration, so
            # that model artifacts do not occur.
            
            random.shuffle(agents)
            agents[i].move()
            agents[i].eat()
            agents[i].share_with_neighbours(neighbourhood)
    
    
    # Looping through the x and ys of each agent and plotting them.
    
    for i in range(num_of_agents):
        matplotlib.pyplot.scatter(agents[i].x, agents[i].y)
    
    
    # Displaying the environment list in a plot.
    
    matplotlib.pyplot.imshow(environment)
    
    
    # Setting the y and x limits of the plot axes between 0 and 99.
    
    matplotlib.pyplot.ylim(0, 99)
	
	
    matplotlib.pyplot.xlim(0, 99)


# Setting up a method for stopping the animation.The animation stops when
# all agents have some food in their store.

def gen_function():
	"""
	Stop the animation and further movement or feeding of the agents once
	all of them have had at least some food.
	"""
    global carry_on
    # If the store is negative or equal to zero, then tield it and carry on
    # with the animation until the store becomes bigger than 0.
     
    if (stored.store <= 0) & (carry_on):
        yield stored.store
        

# Producing an animation of the figure with the different plots each time the
# agents eat and move. The animation is not allowed to be repeated once it
# finishes and the amount of times the agents move and eat have been restricted
# to the number of iterations.

animation = matplotlib.animation.FuncAnimation(fig, update, repeat = False, \
                                               frames = gen_function)


# Displaying the scatter plot.

matplotlib.pyplot.show()


# Writing the amended environment data into a csv file. "newline = ''" stops the
# from leaving one empty row after each one that is being filled in the new file.

with open('dataout.csv', 'w', newline = '') as f2:
    writer = csv.writer(f2)
    for row in environment:
        writer.writerow(row)
```
