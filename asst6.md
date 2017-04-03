# Assignment 6

Gradescope IDs: 605 & 557

All group members have read and followed the guidelines for academic conduct in
CPSC 320. As part of those rules, when collaborating with anyone outside my
group, (1) I and my collaborators took no record but names (and Gradescope
information) away, and (2) after a suitable break, my group created the
assignment I am submitting without help from anyone other than the course staff.

## 1.4 Greedy CEO Part 3

Using the following recurrence relation for _Is Subset Sum_ (ISS)
where A = [a<sub>0</sub>, ... , a<sub>n-1</sub>]:

_ISS(A, k) = ISS( A - [a<sub>n-1</sub>] , k ) ||
    ISS( A - [a<sub>n-1</sub>] , k - a<sub>n</sub> )_

With base cases:

_ISS(A, k) = false_ if k > 0 and length(A) = 0

_ISS(A, k) = true_ if k = 0

In the dynamic programming approach, we build up our subset one _a<sub>i</sub>_
&#8712; A at a time. With the addition of each variable, we determine the
different sums that can be produced with the subset thus far, up to a maximum
of k. This gives a nested iteration over _n_ and _k_, so the algorithm's runtime
_t &#8712; &Theta; (nk)_. This algorithm returns True if the specified subset
problem is satisfiable, and uses 0-based indexing.

```
isSubsetSum(A, k):
    n = size(A)
    S is a 2D array of booleans, size [n+1] x [k+1], initialized to false.

    // Cover base case 1.
    for i in 1 to k:
        S[0][i] = false

    // Cover base case 2.
    for i in 0 to n:
        S[i][0] = true

    // Fill remaining entries in table.
    for i in 1 to n:
        for j in 1 to k:
            // if this sum was possible with a previous subset,
            // it's still possible now. (variable A[i-1] need not be used)
            s[i][j] = s[i - 1][j]

            // this sum is also possible if we can add this variable to
            // a previously possible sum. (variable A[i-1] used)
            if j >= A[i - 1]:
                S[i][j] = S[i][j] || S[i - 1][j - A[i - 1]]

    return S[n][k]
```

An implementation in Python, mostly because we used it to show that our
algorithm didn't break.

```Python
def is_subset_sum(a, k):
    n = len(a)

    # Initialize truth table s.
    s = [[False]*(k + 1) for x in range(n + 1)]

    # Cover base case 1.
    for i in range(1, k + 1):
        s[0][i] = False

    # Cover base case 2.
    for i in range(n + 1):
        s[i][0] = True

    # Fill table using recurrence relation.
    for i in range(1, n + 1):
        for j in range(1, k + 1):

            s[i][j] = s[i - 1][j]
            if j >= a[i - 1]:
                s[i][j] = s[i][j] or s[i - 1][j - a[i - 1]]

    return s[n][k]
```

Explain algorithm to extract a certificate, where S is the solution table
of booleans produced in the solver above. If the specified problem is a
No-instance, the empty list is returned.

```
explainSubsetSum(A, S):
    n = (length of rows in A) - 1
    k = (length of columns in A) - 1

    subset = empty list
    if S[n][k] is false:
        return subset

    i = n
    j = k

    while j > 0:
        while s[i-1][j] is true:
            i--
        add A[i-1] to beginning of subset
        j -= a[i-1]

    return subset
```

Again, in Python:

```Python
def explain_subset_sum(a, s):
    n = len(s) - 1
    k = len(s[0]) - 1

    subset = []
    if not s[n][k]:
        return subset

    i = n
    j = k

    while j > 0:
        while s[i-1][j]:
            i -= 1
        subset.insert(0, a[i-1])
        j -= a[i-1]

    return subset
```

<div style="page-break-after: always;"></div>

# DID NOT CHOOSE
