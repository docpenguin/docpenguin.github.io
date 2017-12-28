```python
# -*- coding: utf-8 -*-
"""
agentframework.py 4.0

Copyright (c) School of Geography.
University of Leeds, Leeds, West Yorkshire, UK, LS2 9JT.

Created on Wed Nov 29 19:08:57 2017

@author: Ourania Sermpezi
"""

#agentframework.py

import random
import math



class Agent():
"""
Override the __init__ method so that x and y values are created randomly for each agent and create accessor
and mutator methods for both x and y. The agents can move, eat and share their stored food with their
neighbours while calculating the distance between pairs of agents.
"""
    # Overriding the __init__ method to take in the lists 'environment' and
    # 'agents' which allows for communication between the various agents in
    # model.py.
    
    def __init__(self, environment, agents):
		"""
		Setting up random x and y values for the agents and setting up environment, agents and
		store variables.
		
		Arguments:
		environment -- the environment around the agents.
		agents -- the agents in the environment.
		"""
        self._x = random.randint(0, 99)
        self._y = random.randint(0, 99)
        self.environment = environment
        self.agents = agents
        # How much the agents have in store.
        self.store = 0
    
    
    def getx(self):
		"""
		Access the value of 'x'.
		"""
        return self._x
    
    
    def setx(self, value):
		"""
		Set the value of 'x'.
		"""
        self._x = value
    
    
    def gety(self):
		"""
		Access the value of 'y'.
		"""
        return self._y
    
    
    def sety(self, value):
		"""
		Set the value of 'y'.
		"""
        self._y = value
    
    
    x = property(getx, setx)
    
    y = property(gety, sety)
    
    
    def move(self):
		"""
		Change the coordinates (i.e. 'x' and 'y'of agents according to random numbers
		and their relationship with the number '0.5'.
		"""
		# If the random number is larger than 0.5, then the y coordinate of the agent
		# increases by one. The opposite happens if the random number is smaller than
		# 0.5. The same code is written for the x coordinate.
		
        if random.random() < 0.5:
            self._y = (self._y + 1) % 100
        else:
            self._y = (self._y - 1) % 100

        if random.random() < 0.5:
            self._x = (self._x + 1) % 100
        else:
            self._x = (self._x - 1) % 100

    
    def eat(self):
		"""
		Agents eat some of the environment around them and according to how much food is
		left in the environment, the store of the agents increases.
		"""
		# If the environment is bigger than 10, then reduce it by 10 and add an additional
		# 10 units to the agent's store.
		
        if self.environment[self._y][self._x] > 10:
            self.environment[self._y][self._x] -= 10
            # Additional 10 units stored for the agent.
			
            self.store += 10
    
    
    def share_with_neighbours(self, neighbourhood):
		"""
		Call the distance_between method to calculate the distance between agents and
		compare it with the neighbourhood variable to decide whether to turn the agents'
		store into the average of their joint stores.
		
		Argument:
		neighbourhood -- each agent's neighbourhood.
		"""
        self.neighbourhood = neighbourhood
        for agent in self.agents:
            distance_agents = self.distance_between(agent)
			# If the distance between the agents is smaller or equal to the neighbourhood,
			# then change both agents' stores to the average of their joint stores.
			
            if distance_agents <= neighbourhood:
                average = (self.store + agent.store)/ 2
                self.store = average
                agent.store = average
	
	
    def distance_between(self, agent):
		"""
		Find the eucledian distance between two agents.
		
		Argument:
		agent -- the other agent that the distance is calculated from.
		"""
	    return math.sqrt(((self._x - agent._x)**2) + ((self._y - agent._y)**2))
```		