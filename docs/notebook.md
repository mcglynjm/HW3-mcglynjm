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

Description:
GraphHandler – This class is responsible for making all graphs and converting between weighted and unweighted.
2DArrayGraph – This class is the representation of a graph using a 2D array.  This gets created and passed a NodeFactory and a EdgeFactory through the constructor.  
ListGraph - This class is the representation of a graph using an ArrayList of Nodes and Edges.  This gets created and passed a NodeFactory and a EdgeFactory through the constructor.  
Node - This interface is implemented by concrete representations of a Node. 
Edge - This interface is implemented by concrete representations of an Edge. 
2DNode – A concrete implementation of the Node interface.  Just holds the name as the 2D graph holds the edges.
ListNode - – A concrete implementation of the Node interface.  Holds a name and a list of outgoing nodes.  
2DEdge – A concrete implementation of the Edge interface.  Holds only the weight of the edge as the start and end are held in the 2DArray.
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
