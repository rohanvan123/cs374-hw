<h1 style="text-align: center;">Backtracking Notes</h1>

## N-Queens

N-Queens problem, which gives all the solutions in which N queens cannot attack eachother on an n x n chess grid

```
def RecursiveNQueens(Q, r):
    n <- len(Q)
    if r == n:
        return Q
    else:
        for j <- 0 to n:
            legal = True
            for i <- 0 to r:
                if (Q[i] == j) or (Q[i] == j + r - i) or (Q[i] == j - r + i):
                    legal = False

            if (legal):
                Q[r] = j
                RecursiveNQueens(Q, r + 1)
```

```
4 x 4 chess grid solutions:
[[1, 3, 0, 2], [2, 0, 3, 1]]
```

The backtracking portion happens after the if (legal) ... where it tests if a position is legal based on the previously placed queens and then does another recursive call. If that branch of recursive calls happens to get to a point where r = n, then n queens have successfully been placed and our solution works. It works just like a DFS.

Runtime:

$$
T(n) = nT(n - 1) + O(n) \approx O(n^n)
$$

## SubsetSum

Given a set X of positive integers and a target T, return true if there exists a subset of X s.t. the elements add up to T.

There are two trivial cases:

$\bullet$ If $T = 0$, then return true becasue $\empty$ is a subset of any set

$\bullet$ if $T < 0$ or $T \neq 0$ and T is empty, then return False

For the general case, consider an arbitrary element $x \in X$ There is a subset of X that sums to T iff

$\bullet$ If there is a subset of X that includes x and whose sum is T

$\bullet$ If there us a subset of X that exclude x and sums to T

Here is the basic algorithm (unoptimized for runtime):

```
SubsetSum(X, T):
    if T = 0:
        return True
    else if T < 0 or X = empty_set:
        return False
    else
        x <- any element of x
        return (SubsetSum(X \ {x}, T) or SubsetSum(X \ {x}, T - x))
```

The base case is if T = 0 or T < 0 or $X = \empty$ assuming that T is does not equal 0. The general case returns true iff $ X \setminus \{x\} $ is a subset that either includes x: $(T - x)$ or excludes x: $T$. We change the value of T because this improves efficiency, it is one of the values we need to keep track of during recursive calls.

The general goal of backtracking algorithms is to store as little as possible during recursive calls (we want to avoid passing in arrays because they are expensive) - instead pass in indices, lengths, etc.

To improve efficiency in this case, it is helpful to chose an $x \in X$ s.t. it is the last element $X[n]$. Hence the subset $X \setminus \{x\}$ is stored in the subarray $X[1...n-1]$.

The more efficient version of our algorithm is:

```
SubsetSum(X, i, T):
    if T = 0:
        return True
    else if T < 0  or i = 0:
        return False
    else:
        return SubsetSum(X, i - 1, T) or SubsetSum(X, i - 1, T - X[i])
```

The running time of this algorithm is
$$T(n) \leq 2T(n - 1) + O(1) \approx O(2^n)$$

This is because at each step we create 2 subproblems of size n - 1, withc O(1) work done at each step

Notice how our answer is polynomial, but our process was exponential (this can be made polynomial via DP)

However, actually generating all the possible subsets (like this problem does is exponential).

## Longest Increasing Subsquence

We need to find the longest sequence whose levels are in increasing order.

Rather than trying to be "smart" our backtracking algorithm will essentially brute force search for the solution

The general cases are to:

$\bullet$ first tentaively add the number and recurse forward

$\bullet$ Then tentatively do not add the number and recurse forward

return the max of the two cases

### What do we need from our past decisions

We can only include $A[j]$ if the resulting subsequence is in increasing order, which we can inductively assume is true for $A[1...j-1]$. We can only include $A[j]$ iff it is larger than the last element selected from the previous elements. This is the only information needed so far.

Hence we cansimplify our problem to:

```
Given an integer prev and an array A[1...n], find the longest increasing subsequence of A in which every element is larger than the preivous
```

Our base case is when we get to the end of the array, which has a length of 0. So the LIS of 0 elements is 0.

The psuedocode for this algorithm is:

```
LISBigger(prev, A[1...n]):
    if n == 0:
        return 0
    else if A[1] <= prev:
        return LISBigger(prev, A[2...n])
    else
        x <- LISBigger(prev, A[2...n])
        y <- 1 + LISBigger(A[1], A[2...n])
        return max(x, y)
```

The code above can be optimized but it generally cuts of the element we just accessed, A[1]. If it is strictly greater than prev, then we take the max of including it (add 1 element) or not including it.

The function is a helper which would be called by the general function as follows:

```
LIS(A[1...n]):
    return LISBigger(-inf, A[1...n]) // sentinal value for prev
```

The running time for this algorithm is:
$$T(n) <= 2T(n-1) + O(1) \approx O(2^n)$$

The logic is that at each index we make 2 decisions, resulting in a overall decision tree with $2^n$ nodes.

## Backtracking Rules

In each recursive call to our Backtracking alg, we need to make exactly one decision, and our choice must be consistent with all previous definitions. Thus each recursive call requires the portion of the input that we have not yet processed as well.

For efficiency the summary or storage of past decisions should be as small as possible. For example for the subset sum problem we pass in both the current Total (this could be altered), which simplifies the problem. Always look for strategies like this to store information

When we design backtracking algorithms we must decide what information we need in the middle of recursive calls in the algorithm (the general case).

$$
$$
