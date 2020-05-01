# ■■■ Day 1
# ■■■ Data structures
# Introduction
A data structure, as implied by the name, is a particular structured way
of storing data in a computer so that it can be used efficiently.

There are 4 classes of computational problems: 
+ P
    any problem that can be solved in polynomial time.
+ NP
    (Nondeterministic Polynomial) 
    where you can `verify the answer` for correctness in polynomial time
    hence P is a subset of NP.
    e.g : subset sum of a set that equals 0
+ NP-hard
    A problem is NP-hard if NP problem can be transformed to it.
    Solution can't be verified in polynomial time
+ NP-complete.
    NP-hard problem that can be verified in polynomial time

P and NP-complete problems are subsets of NP.

If you can deduce that the problem is not in class P you typically are forced
to choose between one of two options:
1. If input size if very small, NP-time solution may suffice.
2. If input size is large, you can try to create a P-time `heuristic`.

## Random numbers
Randomized Algorithms can be broken down into two classes of algorithms:
+ Las Vegas Algorithms, which use the
    `random input` to reduce the expected running time or memory usage but are
    guaranteed to terminate with a `correct result`
+ Monte Carlo Algorithms
    have a chance of producing an incorrect result

It might seem strange, but it turns out that it can be extremely difficult
to achieve true random number generation. The method by which we gener-
ate true random numbers is by measuring some physical phenomenon.

However, there exist computational algorithms that can
produce long sequences of seemingly random results, which are determined
by a shorter initial value, known as a `seed`. These algorithms are much faster
but are pseudo-random (give the same output for the same seed)

# Breadth-first search
BFS solves `shortest path` problem. Graphs are a way to model how diferent
things are connected to one another. 

> BFS
BFS answers `two questions` :
1. Is there a path?
2. What is the shortest path?

BFS uses Queue to save nodes to traverse for later.
You can represent graphs as mapping from one value to multiple values or 
as an object in OOP.

Before checking another object, it’s important to make sure
it haven’t been checked already. To do that, you’ll
keep a list of objects you’ve already checked.

# Tree Structures
## Forest of trees
The general structure of `a set of nodes and edges` is called a Graph
A `tree` is a graph without any undirected cycles nor any unconnected parts.
+ a tree with no nodes is a valid tree called `the null tree`
+ a tree with a single node is also valid tree

All nodes that have children are called internal nodes.

1. rooted binary trees (RBT)
    + All nodes except the root have a parent
    + All nodes have either 0, 1, or 2 children

## Heaps
1. `Priority Queue` is Highest Priority In, First Out (highest priority is first to exit)

There is a specic data structure that developers typically
use to implement the Priority Queue ADT that guarantees
O(n) peeking and O(log(n)) insertion.
A `heap` is a tree that satises the `Heap Property`:
  - for all nodes A and B if node A is the parent of node B,
    then node A has higher or equal priority

The binary heap, has three constraints:
    + Heap property constraint
    + Binary tree property (amount of nodes is 0 - 2)
    + `shaper property` A heap is a `complete tree`, 
        In other words, all levels of the tree, except possibly the bottom one,
        are fully filled, if last level is not complete is't filled from `left to right`.

There are two `types of heaps`: min-heap and max-heap, they differ by prioryty
In min-heap the loves value is at the root of the tree.
In max-heap the greatest value is at the root.

> Operations
`Peek` operation in case of a heap is just taking a look at the root.
`Insertion` 
    1. Insert element in the next open slot in the tree.
    2. We must fix (potentially) disrupted Heap Property, we must
        `bubble up` the newly-inserted element: if inserted element has higher 
        priority than it's parent `swap them` then check if parent has higher 
        priority and if it doesn't `swap again` and so on...
`Removal`
    1. Remove the root
    2. Place the last added element as the root (bottom right element)
    3. Swap the root with the most priority children until it has higher priority 
        than it's children.

`it turns out that we can store a binary heap as an array`
Parent of an element : Math.floor( (i - 1) / 2 )
Left child : 2i + 1
Right child : 2i + 2

## Binary Search Trees
Is a rooted binary tree in which any given node is
`larger than all nodes in its left subtree`
and smaller than all nodes in its right subtree.

> Operations
`The successor function` of a Binary Search Tree finds the next largest node.
Case 1: (if node has a right child)
        Go to the right node.
        Go to the left child recursively until the end.
        This way you find the next value in a sequence.
Case 2: (if node doesn't have a right child)
        Such as we don't have bigger child we have to search among parents.
        We go recursively up the tree until we find a node that is a `left child`
        When we find the node we know that the parent of this node is the successor we
        were looking for.
`Remove`
We must consider three cases: 
    1. Node has no children 
        Just delete it
    2. Has a single child
        Point parent to child directly and remove the node.
    3. Has two children
        replace the node with successor
`In-order traversal`
    To enable traversal from any point you can travers by calling successor repeatedly
    until it returns NULL.
    
a **balanced** k-ary tree is one where most nodes have exactly k children.
nd all leaves are roughly equidistant from the root

## Randomized Search Tree
The Randomized Search Tree is a special type of Binary Search Tree called
a `Treap` (Tree + Heap)

Treap is a binary tree in which nodes contain two items, a key and a priority,
and which must obey the following `two restrictions`:
1. Keys of nodes must follow BST rules
2. Priorities of nodes must follow Heap rules.

How to build a treap?
1. Start with an empty BST
2. Insert in decreasing order of priority using 
    BST insertion algorithm with respect to the keys.

However, this trivial algorithm to construct a Treap relies heavily on
`knowing in advance` all of the elements. 
If we don't know all the elements in advance we can use operations
that allow us to build a tree **dynamically**.

> Operations
`Insertion`
    1. Insert using typical BST insertion algorithm (using key)
    2. Get proper BST, but `priority may be violated`.
    3. Change nodes keeping BST constrains to safisfy Heap constarint, how to do it?
    4. It turns out that we actually are able to "bubble up" a given node without
        destroying the Binary Search Tree properties of the tree via an operation called
        an `AVL Rotation`.

`AVL Rotation`
    AVL Rotations can be done in two directions: `right or left`.
    How to do `right rotation`:
        Imagine we have two nodes A and it's parent B, A has 2 children and 
    B has one more child except A. To `make A a parent of B`
    we have to do the following: 
        A has a `right child` which is bigger than A and less than B
        When we put A in the root it happens to have three children:
        two previous children + B, which is not allowed, so we assign it's 
        previous right children to B which is at the moment our right child.
        Because right children is less than B it becomes it's left child.
    `left rotation`:
        left child of the child element becomes right child of the parent,
        make the parent the child and child the parent.
    How `to rotate TL:DR` :
        0. Save left/right value.
        1. Change direction of child-parent relationship by assigning child's
            left/right node to the parent.
        2. Destroy parent => child relationship on the parent node and
            instead let parent connect to your left/right value that you have saved.

`Removal`
    1. BSD removal
    2. Fix integrity with AVL rotations

`A Randomized Search Tree` is a Treap where, instead of us supplying
both a key and a priority for insertion, `we only supply a key`, and the priority
for the new (key, priority) pair is randomly generated for us.

## AVL Trees


# Hashing 
## Hash tables
A hash function is a function where you put in a string and you get back a number.
+ hash function should return the same result for the same input
+ different input should produce different results (mostly)

> How to use hash function
Get a number => Save value in array with index you get from hash function.
he hash function tells you exactly where the price is stored, so you
don’t have to search at all! Put a `hash function and an array together`,
and you get a data structure called a hash table

> Use cases
Hash tables are great when you want to
+ Create a mapping from one thing to another
+ Look something up
    + Preventing duplicate entries
+ Using hash tables as a cache
    Caching is a common way to make things faster.
    All big websites use caching, and the data is cached in a hash.

**Collisions**
The simplest way to deal with collisions is to create a linked list at the spot
where collision occured. 

**Performance**
`load factor` = N(items) / N(slots), to reduce the load factor you
create a new array that’s bigger. The rule of thumb is to make an
array that is `twice the size`. A good rule of thumb is, resize when your
load factor is greater than 0.7
It’s also very important for hash functions to have a good `distribution`.


# Implementing Lexicon
## Using Multiway Tries (trie stands for retrieval)
a data structure designed for the exact purpose of storing a set of words.

Elements stored in a trie are denoted by the concatenation of the labels
on the path from the root to the node representing the corresponding element.
Nodes that represent keys are labeled as "word nodes"

Trie is a tree where leaves are denoted as word notes.

A trie in which nodes can have more than two edges are known as `Multiway Tries`
We represent edges as letters.

even if you are able to traverse all of the required edges, if the node you
land on is not a `word node` the word does not exist in the trie.
In trie data structure a `letter labels edge, not node`.

## Using Ternary Search Trees













































