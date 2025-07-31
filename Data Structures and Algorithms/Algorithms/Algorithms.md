## Introduction
---
Algorithms are step by step methods to solve a problem computationally. There are different types of algorithms ->
- [[Sorting Algorithms]]
- [[Searching Algorithms]]
- [[Greedy Algorithms]]
- [[Graph Algorithms]]
- [[Dynamic Programming]]
- [[Bitwise Algorithms]]
The way algorithms are written, we have to make sure it efficiently solves a problem fast not using more than needed space. To understand how efficient an algorithm is, we analyse its complexity.
**Note:** $c < log(log(n)) < log(n) < n^{1/3} <n^{1/2} < n<n*logn<n^2 <n^2logn<n^3<n^4<2^n<n^n$ is the order of growth w.r.t an input size of $n$.
## Complexity Analysis
---
Complexity analysis is a technique to evaluate an algorithm based on how much time it takes and how much it space it uses to run when given some input. This is necessary to understand the difficulty of the problem an algorithm is trying to solve as well as to understand its performance and efficiency for larger input size.
### Asymptotic Notations
Different notations are used to denote the complexity of an algorithm, whether it be time or space.
1.  **Big-O Notation** -> It is used to give the complexity for the worst-case scenario of a problem. It sets the upper bound of the resources that is going to be used by the algorithm in terms of space and time. It is written as $O(g(n))$ where $n$ is the input size of the algorithm. (Basically $c*g(n)$ denotes the upper limit of the algorithm with respect to the input)
$$
   O(g(n)) = \{ f(n): there\ exist\ positive\ constants\ c\ and\ n_0\ such\ that\ 0 \le f(n) \le c*g(n)\ \forall\ n \ge n_0 \}
$$
2. **$\Omega$ Notation** -> This is used to give the complexity for the best-case scenario of a problem. It sets the lower bound of the resources that is going to be used by the algorithm in terms of space and time. It is written as $\Omega(g(n))$ where $n$ denotes the input size of the algorithm. (Basically $c*g(n)$ denotes the lower limit of the algorithm with respect to the input)
$$
   O(g(n)) = \{ f(n): there\ exist\ positive\ constants\ c\ and\ n_0\ such\ that\ 0 \le c*g(n) \le f(n)\ \forall\ n \ge n_0 \}
$$
3. **$\Theta$ Notation** -> This gives the average-case complexity which serves as both the upper and lower bound for the algorithm. It is written as $\Theta(g(n))$ where $n$ is the input size of the algorithm. This $g(n)$ represent both the upper and lower limit of the algorithm's efficiency.
$$
\begin{align}
      \Theta(g(n)) = \{ &f(n): there\ exist\ positive\ constants\ c_1, c_2\ and\ n_0\ such\\ &that\ 0 \le c_1* g(n) \le f(n)\le c_2*g(n)
       \forall\ n \ge n_0 \}
\end{align}
$$
   ![[Pasted image 20250730212120.png]]
### Time Complexity
It is the time taken by an algorithm to run as a function of the size of the input but it does not tell you the exact execution time of the algorithm. 
#### Computation
---
- Statements like assignment, comparison, return statements take constant time i.e. their execution time is fixed so we assume they take $O(1)$ or constant complexity
- Complexity of loops are determined by the number of times they run and that is multiplied by the complexity of the block inside it. Eg. nested loops printing a 2D array has a complexity $O(n*m)$ where $n$, $m$ are the number of time the loops run.
### Space Complexity
It represents the memory required by an algorithm to run, as a function of the size of the input although it doesn't represent the exact amount of memory required by the algorithm.
Space complexity includes the size of the input as well as the extra space created during the runtime of the algorithm, which is also called auxiliary space.
#### Computation
---
- If we create a 2D array of $n*n$, the space complexity will be $O(n^2)$.
- In recursion, each call is added to the stack. Hence, stack space is also taken into calculation for space complexity.
  Note: Function calls $\ne$ Stack space as they are not to be stored when they are just being called and it's not recursion. Basically each call takes $O(1)$ space but when its recursion, it's added to the stack to make sure the program knows where it returns to after completion.
### Auxiliary space
It represents the extra temporary memory required for the algorithm to run, like temporary arrays, pointers, variables, etc
Note: Temporary arrays are taken as $O(1)$ until and unless the size of the array depends on the size/length of the input.
### Types of complexity
When analyzing the complexity, there are some standard types of complexity. They are (in ascending order of time taken with $n$ input size) ->
1. **Constant** -> Takes negligible/constant amount of time to execute and is denoted by $O(1)$.
2. **Logarithmic** -> Takes steps in the order of $log_x(n)$ where $x$ is generally 2. Eg. for an array of size 64, it will take around 6 steps to run the algorithm and so on.
3. **Linear** -> Takes steps in the order of $n$ i.e. number of steps similar or close to the size of the input.
4. **Quadratic/Polynomial** -> Takes steps in the order of $n^x$ i.e. where $x$ is a positive real number.
5. **Factorial** -> Takes steps in the order of $n!$.
6. **Exponential** -> Takes steps in the order of $k^n$ where $k$ is a positive real number.
