# ■■■ Day 1
# Data structures
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
1. `Priority Queue` is "Highest Priority In, First Out"(highest priority is first to exit)

There is a specic data structure that developers typically
use to implement the Priority Queue ADT that guarantees
O(n) peeking and O(log(n)) insertion.
A `heap` is a tree that satisfies the `Heap Property`:
  - for all nodes A and B if node A is the parent of node B,
    then node A has higher or equal priority

The binary heap, has three constraints:
    + Heap property constraint
    + Binary tree property (amount of nodes is 0 - 2)
    + `shape property` A heap is a `complete tree`, 
        In other words, all levels of the tree, except possibly the bottom one,
        are fully filled, if last level is not complete is't filled from `left to right`.

There are two `types of heaps`: min-heap and max-heap, they differ by priority
In min-heap the lowest value is at the root of the tree.
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
    3. If nucessary, bubble down root with children that have higher priority.

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
    To enable traversal from any point you can traverse by calling successor repeatedly
    until it returns NULL.
    
a **balanced** k-ary tree is one where most nodes have exactly k children.
and all leaves are roughly equidistant from the root

# ■■■ Day 2
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
    2. You get proper BST, but `priority may be violated`.
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
(Adelson-Velsky and Landis tree)

An AVL Tree is a Binary Search Tree in which, for all nodes in the tree, the
heights of the two child subtrees of the node differ by at most one. If two
subtree heights differ by more than one `rabalancing is done`.

1. Balance Factor of a node
    difference in height between two subtrees
2. Balance factor in AVL tree for all nodes must be less than 2

AVL rotations change the number of children in the node.
Node that is a child gives one it's child to the parent.

> Operations
`Insertion`
    1. Regular BST insertion
    2. Go up the tree and update all balance factors.
    3. For parent nodes that are out of balance, perform AVL rotation with it's child.
`Removal`
    1. Regular BST removal
    2. Go up the tree and update the balance factor.
    3. Perform AVL rotations when needed.

Because of this phenomenon where the `tree balances itself` without any user guidance,
we call AVL Trees "self-balancing."

**Note** sometimes a single rotation fails to balance the tree.
To combat this problem, we will introduce the `double rotation`:
    1. Transform a kink (curve) into a more straight line with one rotation
    2. Balance the staright line with another rotation.
```
/
\
then
 /
/
then 
/\
```
You need to perform double rotation when you have a parent that is unbalanced
(balance factor of 2) and a child that is `slightly unbalanced` (BF = 1)
but in another direction (so you get the kink `>`)

## B-Trees
Until now we have been assuming that all node accesses take that same amount of time.
Which is not true (processor registers are 10x faster than RAM(5ns vs 50ns))

The sections of memory in processor that are fast are small in size consequently,
as our data structures grow in size, they will not be able to fit in such
a small section of memory.

Processor loads data that is spacially close to each other.
(e.g array values will be close spacially in processor cache)

That means that to speed up our trees we need to minimize the number of traversals
and `store as much data as possible close together`

By having a node store multiple keys, we are able to trick the CPU into loading more than
one key at once into a fast section of memory (L1 cache)

A `B-Tree` is a self-balancing tree data structure that generally allows
for a node to have more than two children 
(to keep the tree wide and therefore from growing in height)

`Order b` of a b-tree is the number of children any node is allowed to have.
2b is the maximum number of children any internal node is allowed to have.

B-Tree should satisfy the following properties:
1. Every node has at most 2b children
2. Every internal node has at least b children
3. An internal node with k children contains k - 1 keys.
4. All leaves appear in the same level.

> Operations
`Insertion`
    The secret to having the leaves constantly be on the same level is
    to have the `tree grow upward` instead of down from the root
    
    You insert values into a node, when you have more than 2b - 1 values in a node
    get an item in the center of the ordered array that contains values
    and create a new parent node with this value, you already have a parent
    node, add to it this value, if its full, take the middle value from there and 
    go upwards again to create parent/add values to existing.
`Deletion`
    When deleting you need to consider many cases:
        1. Deleting a leaf value - just delete
        2. Deleting non-leaf value 
        3. Delete the last value in a child node 
        4. etc.

# ■■■ Day 3          
# Introduction to Graphs
Graph consists of a set of edges and nodes (vertices).
Graph can be represented as two sets (V, E) where V is a set of vertices 
and E is a set of edges. G = (V,E)

A `weighted graph` is a graph in which each edge has a weight
A `cycle` in a graph is a valid path that starts and ends at the same node.

## Graph Representations
If every node in a graph is connected with every other node it has (n-1)^2 edges.

You can represent a graph as a table where x and y are both complete lists
of vertices and 1 or 0 put in some x,y coordinate means that there's an edge
coming from x vertice to y.
In weighted graphs you put number representing a weight instead of 1 and null instead of 0

Table takes n^2 space which may be redundant for `sparse graphs`
so you can represent graphs in another way using `adjacency list`
which is a list of vertices and for every vertice you have a list of vertices
this vertice is connected to:
```
a: {b, c}
b: {e, f}
```

## Breadth First search
Currently there's no algorithm that can calculate shortest path to a single 
vertice without calculating path to every other vertice. The best optimisation we 
can harness is to "quit early" when we see that this path obviously is not the most 
optimal.

One `algorithm to explore all vertices` in a graph is called Breadth First Search
But it may consume a lot of computer memory because it uses Queue.

## Depth first search
The idea behind DFS is to explore all the vertices going down
from the current vertex before moving on to the next neighboring vertex.
recursive nature of the algorithm _provides the stack implementation for us_.
DFS can be more memory efficient in certain cases.

## Dijkstra's Algorithm
The idea behind Dijkstra's Algorithm is that we are constantly looking for the
lowest-weight paths and seeing which vertices are discovered along the way.

Dijkstra's Algorithm needs to just
1. Search for the next shortest path that exists in the graph
2. See which vertex it discovers by taking that shortest path.

# Greedy algorithms
How to find biggest set of classes that overlap? Just keep picking those that 
end the soonest. A greedy algorithm is simple: `at each step, pick the optimal move`.

## Knapsack problem
just pick the most expensive item every time.

Sometimes all you need is an algorithm that solves the
problem good enough. And that’s where greedy algorithms shine

`Dijkstra's` algorithm is a `greedy` algorithm. It always looks at the shortest path.

# Hashing 
## Hash tables
A hash function is a function where you put in a string and you get back a number.
+ hash function should return the same result for the same input
+ different input should produce different results (mostly)

> How to use hash function
Get a number => Save value in array with index you get from hash function.
The hash function tells you exactly where the price is stored, so you
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
create a `new` array that’s bigger. The rule of thumb is to make an
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

# ■■■ Day 4
# Javascript Algorithms and Data Structures >>>>>
# Memory management
## Memory leak
A memory leak is a failure in a program to release discarded memory
Access to a reference to an object loads the whole object into memory.

## Leaking DOM
When you delete an element from html you should make sure that there are no
other variables in your code that reference that html element, otherwise the 
element will be deleted in html but still remain in memory, because some variable 
points to it.  

# Recursion
## Fibonacci with tail recursion
A tail recursive function is a recursive function in which the recursive call is the last
executed thing in the function.  
```js
function fibonacci(n, lastlast, last) {
    if (n == 0) {
        return lastlast;
    if (n == 1) {
        return last;
    return fibonacci(n-1, last, lastlast + last);
}
```
Recursive functions have an additional space complexity cost that comes from the
recursive calls that need to be stored in the operating system’s memory stack.

# Sets
Set is a group of definite distinct objects. Sets are `based on hash tables`
and have checking and adding a unique element in O(1).
```js
let set = new Set();
```
Sets have only one property - `size`, which represents current number of objects in set.

## Operations
Set has one primary function: to check for uniqueness.
```js
var exampleSet = new Set();
exampleSet.add(1); // {1}
exampleSet.add(1); // {1} <= duplicates are silently ignored.
exampleSet.add(2); // {1,2}
exampleSet.delete(1); // {2}
exampleSet.has(1); // false
exampleSet.has(2); // true

/*utility functions*/
function setIntersection(set1, set2) {
    /*you need to transform a set into an array to access array's methods*/
    return [...set1].filter(elem => set2.has(elem));
}
function isSupersetOf(super, sub) {
    return [...sub].every(elem => super.has(elem));
}
function unite(set1, set2) {
    const newSet = new Set(set1);
    for(let elem of set2) 
        newSet.add(elem);
    return newSet;
}
function XOR(set1, set2) {
    // const xor = new Set();
    // for(let elem of [...set1, ...set2])
    //     if( !(set1.has(elem) && set2.has(elem)) ) 
    //         xor.add(elem);
    // return xor;
    return [...set1, ...set2].filter(e => !(set1.has(e) && set2.has(e)));
}
```

# Searching and sorting
## Sorts
> Selection
1. Find smallest
2. Unshift

> Insertion
1. Take next
2. Find place to insert
(some implementations use "bubbling" technique where you take next item swap it
with items you have until the item finds it's place)

> Count sort O(n)
This sort doesn't compare values and works only for integers of a `small range`.
How it works? It counts number of occurences of every number in an array and 
then recreates array from scratch by putting numbers in an array the amount of 
times it saw it before.
```js
[3,1,3,1,3,1,1]
// count table
{
    "0" : 0
    "1" : 4
    "2" : 0
    "3" : 3
}
 
[0,1,1,1,1,3,3,3]
```

## Exercise
1. sqrt without any math library
    1. Use binary search
```js
function getSqrt(n) {                             // sr -> square root
    if(n == 1 || n == 0 ) return n; 
    if(n%1 != 0) 
        throw new Error("Only integers allowed"); // base cases
    
    let sr = 1;
    let learning_rate = 2;
    
    while((sr**2).toFixed(5) != n) {
        let try_sr = sr + learning_rate;
        if(try_sr**2 < n) 
            sr = try_sr;
         else 
            learning_rate /= 2;
    }
    return sr;
}
```

# ■■■ Day 5
# Trees
## Tree traversal
1. Pre-order traversal (depth-first parent first)
2. In-order traversla (form the smallest number to the biggest
    (using `successor` finding algorithm)
3. Post-order traversal (depth-first childrent first)
4. Level-order traversal (breadth-first)

## Heap sort
Pop from heap repeatedly.
`O(n * log(n))` in theory as fast as you can get, but in practice not that fast.

# Strings
## Trie(prefix tree)
Trie is a tree that has it's leaves as ending letters of words.
To delete a word you just go the the leaf of the word, and then go 
upwords deleting until there's a node with more than one child.
(to not to accidentally delete another word)

## Boyer-moore string search
For each substring that you want to find in some string you need first to 
create a match table for each letter which will specify how many characters you can skip
in case the string you compare your substing to doesn't contain the letter.
e.g : `jam {j: 2, a: 1, m: 3}`
Then each time you move you see if substring has characters from the table.

# CTCI
Check uniqness of characters in a string without hastable.
```js
function unique(string) {
    if(string.length < 3)
        return string[1] != string[2];    
    
    let suffix = string.slice(1);
    let letter = string[0];
    return without_letter(suffix, letter) && unique(suffix);
}

function without_letter(str, letter) {
    let result = true;
    for(let i = 0; i < str.length && result; i++) {
        if(str[i] == letter)
            result = false;
    }
    
    return result;
}

// another solution is creating an array from string sorting it 
// and searching for equal subsequent characters

// another solution
const uc = c => str => !!~str.indexOf(str);
const us = str => [...str].reduce((b,c,i) => b && uc(c)(str.slice(i)), true);
// b - base, c - character, i - index, uc - uniq character, us - unique string
```

Substitute spaces with `%20`.
```js
function substituteSpaces(string) {
    let newString = '';
    let lastChar = null;
    
    for(let c of string) {
        let substituteSymbol = lastChar == ' ' ? '':'%20';
        newString += c == ' ' ? substituteSymbol : c;
            
        lastChar = c;
    }
    
    return newString;
}

// or
const subc = a => b => c => c == a ? b : a;
const sub = a => b => str => [...str].map(subc(a)(b));
const converter = sub(' ')('%20');
const newStr = converter(oldStr);
```

Check that whether a string is a randomized polindrome.
```js
function checkForPolindrome(string) {
    let table = {};
    let numberOfOddNumbers = 0;
    
    for(let c of string) {
        if(!table[c])
            table[c] = 1;
        else 
            table[c]++
    }
    
    for(let c in table) {
        if(table[c] % 2 > 0)
            numberOfOddNumbers++;
    }
    
    return numberOfOddNumbers < 2;
}

//or
const odd => c => str => [...str].filter(ch => ch == c).length % 2 == 1;
const polindrome = str => [...str].reduce((b,c) => odd(c)(str) ? ++b : b, 0) < 2;
```