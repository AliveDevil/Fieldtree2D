# Fieldtree2D
Cover (loose) fieldtree and Partition fieldtree structures for 2D geometries for .Net.

# Installation
Nuget package is uploaded to: https://www.nuget.org/packages/FieldTree2D/1.0.0
The package can be installed by using following command:

   Install-Package FieldTree2D -Version 1.0.0 

# Introduction
Cover Fieldtree and Partition Fieldtree are designed for efficient storage of 2D spatial objects (such as rectangles, circles, ...).

Cover (loose) field tree is similar to MX-Quadtree or PR-Quadtree, where each node can have 4 children, and partition occurs at the center of the node. Unlike PR-Quadtree and MX-Quadtree, Cover Fieldtree accepts a p_value which is a floating point number between 0 and 1 (0 < p_value < 1). This value is being used to enlarge the nodes by (1 + p_value) times for better storage efficiency. p_value = 0 is known as MX-Quadtree.

In Partition Fieldtree, the children are created by shifting them by 1/2 their size both in X and Y direction. In this scenario each node can have 9 children and up to 4 parents (possible number of parents are 1 for center node, 2 for edge nodes, and 4 for corner nodes).This dependency causes more computation during adding children and merging them (upon delete), while give the most effiecient storage and find operations. 

# Usage
The supported objects for storage in the data structure need to implement `ISpatial` interface:

  `using FieldTreeStructure.Spatial;`
  `public class Rect : ISpatial {}`

Then data structre can be initialized using

  `CoverFieldTree<Rect> coverFieldTree = new CoverFieldTree<Rect>(width, height, p_value);`
  
or

  `PartitionFieldTree<Rect> fieldTree = new PartitionFieldTree<Rect>(width, height);`
  
Objects can be added to the data structure using `Add` or `AddMany`. Objects can be removed using `Remove` or `PopNearestObject` where the latter finds the object closest to the specified query point and pops it. 

Point Query without deletion can be performed using `FindNearestObjects`.

Range Query can be performed using `RangeQuery` for circular or rectangular sections.

To retrieve all object `All` can be used.

Both data structures support incremental nearest neighbor finidng. To use this feature, first initialize the query point using `InitIncrementalNearestNeighbor` and then find the next closest object by calling `IncrementalNearestNeighborFindNext`. This algorithm doesn't reset the search, therefore, it can be used to efficiently sort object based on closeness to the query point.

To reset the data structure `Clear` command can be used.

