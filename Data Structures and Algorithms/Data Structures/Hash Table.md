## Introduction
---
Hashing is a process of mapping an element/data to a specific index through the means of a hash function, which gives a certain output for a certain input. This enables fast access to information based on the element's key. Algorithms like searching, insertion and deletion on this data structure has a time complexity of $O(1)$. This is used for key-value pairs.
## Hash Function
---
Hash functions are mathematical functions which take in an input and give an output according to it. It should give an unique output for every unique input.
Hash functions should be:-
1. Efficient
2. Near to no collisions i.e. collisions should be minimized as we can't construct perfect hash functions for large number of inputs.
3. Uniform distribution of keys in hash table.
4. Low load factor. (not greater than 0.75)
## Load Factor (?)
---
This tells us about the efficiency of the hash function in distributing the keys uniformly across the hash table. It is defined as the 
$$
\frac{Number\ of\ items}{Size\ of\ Hash\ table}
$$
## Rehashing (?)
---
Rehashing is basically the process of hashing the input again. When the load factor increases ($\gt0.75$), the complexity increases. Hence, the hash table size is doubled and the keys rehashed to maintain a low factor and low complexity.