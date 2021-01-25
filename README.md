# DSA-With-Ruby
==============

A collection of data structures in Ruby, I made to learn Ruby in short time.

## List of data structures

1. queue
2. stack
3. tree
4. linked list
5. adjacency list (graph)
6. deque
7. priority queue
8. priority deque
9. binary tree

## Queue

A queue is a simple container-based structure that mimics a real-life queue (e.g. waiting in line at the bank). It is FIFO (first-in-first-out), meaning that when you retrieve items from the queue, they are returned in the order in which they entered. Ruby Arrays provide methods that make Queue implementation trivially easy, but having them named appropriately and contained in a convenience class is worth it to see how they are implemented, and because other structures will inherit from this one. An alternate implementation could be done using a linked list.

Usage:

```ruby
require 'datastructures'
queue = DataStructures::Queue.new
queue.enqueue('first')
queue.enqueue('second')
queue.size # => 2
queue.empty? # => false
queue.front # => 'first'
queue.back # => 'second'
queue.dequeue # => 'first'
queue.dequeue # => 'second'
queue.dequeue # => RuntimeError, "Queue underflow: nothing to dequeue"
```

## Stack

The stack is the sibling of the queue. It mimicks a real-life stack (e.g. of paper). It is FILO (first-in-last-out), so that when items are retrieved from the stack, they are returned in the reverse of the order in which they were added. Again, Ruby Arrays provide a perfect container. As with the Queue, it could also be implemented using a linked list.

Usage:

```ruby
require 'datastructures'
stack = DataStructures::Stack.new
stack.push('first')
stack.push('second')
stack.size # => 2
stack.empty? # => false
stack.top # => 'second'
stack.bottom # => 'first'
stack.pop # => 'second'
stack.pop # => 'first'
stack.pop # => RuntimeError, "Stack underflow: nothing to pop"
```

## Tree

A tree is a directed graph where any two nodes are connected by only one edge, and there is a specified 'root' node, away from which all edges are directed. For any edge, the node that it points from is the *parent*, and the node it points to is the *child*. This implementation is currently very crude, and I hope to expand it by:

- separating the node and tree classes
- including search and shortest path algorithms
- include more traversal algorithms

Currently the implementation offers the ability to construct a tree and traverse it manually using family methods (*parent*, *child*, *siblings*, *descendents*). Nodes can have associated data.

```ruby
require 'datastructures'

# start with a root TreeNode
tree = DataStructures::TreeNode.new('root node')
tree.is_root? # => true
tree.is_leaf? # => true

# children are also TreeNodes
child1 = DataStructures::TreeNode.new('child')
tree.add_child(child1)
tree.child_count # => 1
tree.add_child(DataStructures::TreeNode.new('another child'))
tree.child_count # => 2
tree.is_root? # => true
tree.is_leaf? # => false
child1.is_leaf? # => true

# we can traverse by querying the family
tree.siblings # => []
child1.siblings.map{ |sibling| sibling.data } # => ["another child"]

child1.parent == tree # => true
child1.parent == child.siblings.first.parent # => true

tree.descendents.map { |d| d.data } # => ["child", "another child"]
```

## Linked List

A linked list is a group of items which are ordered, and where the ordering is determined solely by the information each item contains about its neighbours.

This implementation is a doubly-linked list, which means that each item retains a reference to the *next* and *previous* items in the list.

The advantage of a linked list over a traditional array is that elements can be inserted and deleted efficiently.

```ruby
ll = DataStructures::LinkedList.new('one')
ll.first.data # => 'one'
ll[0] # => 'one'
ll << 'two' # => ["one", "two"]
ll.size # => 2
ll.length # => 2
ll.last.data # => 'two'
ll[ll.size - 1] # => 'two'

# linked lists can act as stacks
ll.push 'three' # => ["one", "two", "three"]
ll.size # => 3
ll.pop # => "three"
ll.size # => 2
puts ll # => ["one", "two"]

# or as queues
ll.push 'three' # => ["one", "two", "three"]
ll.shift # => 'one'
ll.size # => 2
ll.first.data 'two'

# we can insert and delete
ll.insert(1, 'one point five') # => ["two", "one point five", "three"]
ll.delete(1)
ll.to_s # => ['two', 'three']
```

## Deque

A Deque is a queue which allows adding and removing items at both ends.

```ruby
require 'datastructures'

dq = DataStructures::Dequeue.new
dq.enqueue('first') # => ['first']
dq.enqueue('second') # => ['first', 'second']
dq.front_enqueue('third') # => ['third', 'first', 'second']
dq.size # => 3
dq.empty? # => false
dq.front # => 'third'
dq.back # => 'second'
dq.dequeue # => 'third'
dq.back_dequeue # => 'second'
dq.dequeue # => RuntimeError, "Queue underflow: nothing to dequeue"
```

## Priority Queue

A Priority Queue is a queue where items can be added at arbitrary positions in the queue.
Dequeuing always returns the item with the highest priority.

```ruby
pq = DataStructures::PriorityQueue.new
pq.enqueue('first', 0) # => ['first']
pq.enqueue('second', 1) # => ['second', 'first']
pq.enqueue('third', 0) # => ['second', 'first', 'third']

# if priority is omitted, defaults to 0 (so items get added to the back of the queue)
pq.enqueue('fourth') # => ['second', 'first', 'third', 'fourth']
```

## Priority Deque

A Priority Deque is a deque where items can be added at arbitrary positions in the queue.
Dequeueing can return either the highest or lowest priority item depending on the method used.

```ruby
pd = DataStructures::PriorityDeque.new
pd.enqueue(1, 0) # => [1]
pd.enqueue(2).enqueue(3).enqueue(4) # => [1, 2, 3, 4]
pd.enqueue(5, 2) # => [5, 1, 2, 3, 4]
pd.dequeue # => 5
pq.back_dequeue # => 4
```

## Binary Tree

A binary tree is a tree in which each node can have a maximum of two children. The children are designated _left_ and _right_. All TreeNode methods are available except _:add_child_ and _:remove_child!_.

```ruby
bt = DataStructures::BinaryTreeNode
root = bt.new(1)
leftchild = bt.new(2)
rightchild = bt.new(3)
root.add_left_child leftchild
root.add_right_child rightchild
root.children # => [2, 3]
```
