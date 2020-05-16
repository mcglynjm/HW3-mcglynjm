HW3
Entry1: 5/5/20
Drivers Given:
Primary functionality:
 • US1: As a researcher, to identify the optimal shortest path implementation to use for a given graph, I want to compare the performance of an algorithm’s 2D Array implementation with its corresponding List implementation. 
• US2: As a researcher, to identify performance bugs in a shortest-path implementation, I want to compute the average execution time of the implementation in different situations. 
• US3: As a researcher, to compare the performance characteristics of algorithms on weighted and unweighted graphs, I want to easily convert an existing weighted graph to an unweighted graph. 
Quality attribute scenarios:
• QAS1: When a researcher tests an existing algorithm implementation on a new graph, the system shall enable the researcher to add the new graph to the system with no modifications to the existing code. 
• QAS2: When a researcher adds a new algorithm implementation, the system shall enable the researcher to add the new implementation with no modifications to the existing code. 
Concerns 
• CRN1: The performance difference between shortest path implementations that use 2D Arrays vs Lists matters a great deal. We must achieve a balance of abstraction that makes the design maintainable yet preserves the performance properties of both data structures so that the simulation’s timing data are accurate. 
• CRN2: Graph algorithms sometimes modify the graph. We must regenerate the graph from scratch each time we test a shortest path algorithm on it.

Drivers to Address:  
CRN1, QAS1, CRN1

Factory Method Design:
We need a GraphHandler abstract class with an abstract createGraph() method.  We need two concrete subclasses  of the GraphHandler class for list and 2D array graphs.  We will also need a Graph abstract class with two concrete classes that subclass it.  We can then implement the createGraph() in each of the creators.  This design allows for us to allow both types of graphs to rely on one abstraction.  We can also easily handle changing to an unweighted graph as we can add an Edge interface for unweighted and weighted edges.  It also allows us to get the specific functionality that varies between the 2D array and list graphs while still relying on interfaces or abstract classes.  
Pros:
Easy to update
Reliance on interfaces 

Cons:
Loss of implementation details that are important in this application

Abstract Factory Design:
For this design we will not have the common Graph abstraction.  For this we will need to take the factory method design and modify it.  We will have another interface called EdgeFactory that will take care of creating the needed components for the graph.  This interface will be implemented by 2DEdgeFactory, ListEdgeFactory, 2DUnweightedEdgeFactory, and ListUnweightedEdgeFactory.  This allows us to return the specific type of graph as we no longer have an interface.  This design seems to favor keeping the graphs in their separate implementation forms as is mentioned in CRN1.  
Pros:
Timing will be more accurate based on implementation 
Better at following the SRP
Easy to switch to unweighted graph
Divides the graphs up into more related families
Cons:
Less reliance on interfaces
Less dependency injection due to the loss of the Graph interface 
More complex


Overall I prefer the Abstract Factory design.  It offers much more in the terms of allowing us to keep the implementation details as much as we could.  While the lack of the Graph interface could be annoying when it comes to being stuck with the concrete types this may be insignificant as the data part of the problem doesn’t seem like it could expand very much.  This design also allows for easily regenerating a graph as you just call the EdgeFactory with the original nodes and edges from the GraphHandler.  

Class Diagram:
 <img src = 'https://github.com/mcglynjm/HW3-mcglynjm/blob/master/images/AbstractFactory.png' width = '640'/>
Description:

GraphHandler – This class is responsible for making all graphs and converting between weighted and unweighted.

2DArrayGraph – This class is the representation of a graph using a 2D array.  This gets created and passed a NodeFactory and a 

EdgeFactory through the constructor.  

ListGraph - This class is the representation of a graph using an ArrayList of Nodes and Edges.  This gets created and passed a 

NodeFactory and a EdgeFactory through the constructor.  

Node - This interface is implemented by concrete representations of a Node. 

Edge - This interface is implemented by concrete representations of an Edge. 

2DNode – A concrete implementation of the Node interface.  Just holds the name as the 2D graph holds the edges.

ListNode - – A concrete implementation of the Node interface.  Holds a name and a list of outgoing nodes.  

2DEdge – A concrete implementation of the Edge interface.  Holds only the weight of the edge as the start and end are held in the 
2DArray.

ListEdge - A concrete implementation of the Edge interface.  Holds the weight of the edge and the start and end of the edge.

UnweightedListEdge – A concrete implementation of the Edge interface.  Does not hold the weight and just returns 1 for getWeight().

NodeFactory – An interface for creating nodes.

2DNodeFactory – A concrete implementation of the NodeFactory interface.  Just makes an array of 2DNodes.

ListNodeFactory - A concrete implementation of the NodeFactory interface.  Just makes an ArrayList of ListNodes.

EdgeFactory – An interface for creating edges.

2DEdgeFactory – A concrete implementation of the EdgeFactroy interface.  Makes the 2D array for the edges and weights of a 2DArrayGraph.  Fills in empty spaces with max integer.

2DUnweightedEdgeFactory - – A concrete implementation of the EdgeFactroy interface.  Makes the 2D array for the edges of a 2DArrayGraph where all edges are weight 1.  Fills in empty spaces with max integer.

ListEdgeFactory – A concrete implementation of the EdgeFactroy interface.  Makes the ListEdge ArrayList for the edges and weights of a ListGraph.

ListEdgeFactory – A concrete implementation of the EdgeFactroy interface.  Makes the UnweightedListEdge ArrayList for the edges of a ListGraph with all weights as 1.

This took about 4.5 hrs.

Entry 2: 5/6-7/20
Drivers:
US1
US2
US3

Design from Domain Model:
From the domain model the important objects for the domain layer will be the ShortestPathAlgorithm the ShortestPath and the Graph.  Using the last design we should include the GraphHandler in the domain layer.  Each algorithm could have an instance of a Result class that holds the shortest path and the time taken.  We can then have an Experiment class that composes the GraphHandler and a ShortestPathAlgorithm and a 2DRunner and a ListRunner to handle running the different implementations of the graphs.  Experiment will create the graph and hold all the algorithms.  When testImplementation() is called it will call the 2DRunner’s runAlgorithm () with the graph and an algorithm.  It will return a Results class with the path and the time taken.  Then the Experiment does the same with the ListRunner.  We can do the same thing for finding the average time but loop through calling the experiment with different graphs but the same algorithm.  Then we can take the time from the Results class and find the average.  Experiment will have the methods testAlgorithm(), findAverage(), and convertToUnweighted.

Design from Transaction Scripts:
For this design we will have one function per User Scenario.  We will have a function to compare the performance of an algorithm on the 2D and List implementations.  We will have a function to find the average execution time of an implementation.  We will have a function to convert to an unweighted graph.  For this design we can only have one instance of an object.  We will have one static instance of a GraphHandler class, one static instance of a CompareImplementations class, one static instance of a FindAverage class, one static instance of a ConvertToUnweighted class, and one static instance of a Results class.  However we will need to be careful and make sure that the code is safe for multithreading.  

Overall I prefer design 1.  Due to the fact that I chose the design without the common Graph abstraction this singleton pattern does not work as well as I need two methods for doing things to the different kinds of graphs.  While this will produce more classes it will produce classes with less duplicate code.  

Class Diagram:

<img src = 'https://github.com/mcglynjm/HW3-mcglynjm/blob/master/images/DomainDesign.png' width = '640'/>


Description:

GraphHandler – Same as before.

Experiment – Class  in charge of running the three user scenarios for comparing implementations, averaging time, and converting to unweighted.  To accomplish this it uses the GraphCreator and the Runner classes.

Results – This class stores the shortest path and the time taken.  Used to pass results back and keep them separate to allow more things to be added to the Results if needed.

ListRunner – In charge of running the algorithms on the ListGraph implementation.

2DRunner - In charge of running the algorithms on the 2DArrayGraph implementation.

2DShortestPathAlgorithm – Interface for all shortest path algorithms operating on a 2DArrayGraph.  The concrete algorithm classes are left out to save space.

ListShortestPathAlgorithm – Interface for all shortest path algorithms operating on a ListGraph.  The concrete algorithm classes are left out to save space.

ListGraph – Same as before.

2DArrayGraph – Same as before.

This took about 4 hrs.

Entry 3 5/8-9/20:
Design 1:
For adding more ShortestPathAlgorithm methods we can apply the strategy pattern.  We can add another interface called ShortestPathAlgorithm that both 2DShortestPathAlgorithm and ListShortestPathAlgorithm extend.  We can do the same thing with a common Runner interface for 2DRunner and ListRunner.  We give the runners a ShortestPathAlgorithm field in the constructor and a setAlgorithm() method to change the algorithm if needed.  This allows us to choose the algorithm at runtime  and add more without modifying the existing code as any new algorithms will implement the ShortestPathAlgorithm interface.  
Pros:
Easy to add more methods
Can do lazy instantiation
Less reliance on implementation due to interfaces
Cons:
Due to splitting the two types of graphs we add another layer of abstraction that may be too much

Design 2:
We can add more methods using a factory pattern.  We keep the current structure but add in a AlgorithmFactory class that the Experiment uses to create every method when the experiment is started.  This would allow for adding new methods easily while adding a layer of abstraction between the Experiment and the creation of the algorithms.  
Pros:
Easy to implement 

Cons:
More costly with computing power
More costly with space

Overall I prefer design 1.  Design 1 allows for more modifiability especially when changing a method at runtime.  

Class Diagram:

<img src = 'https://github.com/mcglynjm/HW3-mcglynjm/blob/master/images/Strategy.png' width = '640'/>


Sequence Diagram for some algorithm MyAlgorithm:


<img src = 'https://github.com/mcglynjm/HW3-mcglynjm/blob/master/images/sequence.png' width = '640'/>


This took 3 hrs.

Entry 4: 5/13-16/20

Class Diagram:

<img src = 'https://github.com/mcglynjm/HW3-mcglynjm/blob/master/images/ClassDiagram.PNG' width = '640'/>

Sequence Diagram:

<img src = 'https://github.com/mcglynjm/HW3-mcglynjm/blob/master/images/SequenceDiagram.PNG' width = '640'/>

Problems encountered while implementing:

Why do the unweighted graphs all have 0 weight.  Shouldn’t it be 1.  No clear way to change to an unweighted graph (added a method in the Transaction Script and factory).  Also to use the graphs remakeMe() to call the factory don’t we need an instance of the factory and to save the original specification in the graph.  Why are the fields defined in the interface?  Storing edges as strings and not as a separate class caused lots of trainwrecks and extra functionality in the ListGraph.  Needed to add methods to each graph interface to access the data structures in the graphs.  The design will not scale well with adding more methods.  The return types of the algorithms are not specified.  The lack of an edge class leads to ugly string parsing in the list graphs.  While it was mentioned it was never shown in the diagrams so I did it without them.  

Questions:

a. The peers design did not function due to factors mentioned above.

b. This design tries to favor composition over inheritance however due to the poorly set up factories and graphs it gets very ugly.  An edge class would make this so much better.

c. The design does not program to interfaces in the graph.  They somehwat program to interfaces with the factories although I'm not sure why they chose to make a factory for files.  Just make one for each type of graph.

d. As mentioned above there are many train wrecks.  Some of these come from having the edges as strings.  

e. The design was fairly cohesive especially with the TransactionScript class.  However this specific class does violate SRP.  

f. The design notebook lacked a lot of specifity on things like converting to unweighted, how the graph can call the factory, and other things such as return types and where variables are stored.  The design was also lacking in the description of the diagrams presented making implementation a painful 20+ hr process (sticking to the design and not using edge classes).

g. In my design some more specificaiton on return types would be useful.  Also I realized I forgot to cite the textbook for the patterns.

This took 21 hours.
