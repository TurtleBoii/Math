# The Sub set sum NP problem:
-----------------------------------------------
Given a set C $\epsilon$ $\Zeta$. For a value T, check if any subset of integers sum up to T.

## Approach:
-----------------------------------------------
The hypothesis is based on three main checks. Which of course have to be mathematically proven, but I haven't done it yet lol. We'll start by looking at a set of numbers.

C = {2, 6, -10, 4, -1, -7, -72, 19}

We can use any sorting algorithm, at least the effective ones (don't use random sort, I mean, obviously). We'll do as if the set was sorted with the Radix sort algorithm for it to be O(log(n)).

Then, C will be sorted into an array in the following order:

C = [-72, -10, -7, -1, 2, 4, 6, 19]

While the array gets sorted, we will be saving the sums of negative and positive values on two separate variables, posSum and negSum. In the end, 
posSum = 31 and negSum = -90.

Given the value T = 7, we will follow the next steps:

### Step one - Relation between positive values and T.
------------------------------------------------------
This first check will confirm if the sum of all the positive elements in C is equal or greater than T. The following conditions will then happen:

> If posSum = T, then we have found a subset of C that complies!

> If $posSum \geq T$, then there may be a subset of C that complies.

> If $posSum<T$, there there are no subsers of C that comply. It's easy to verify. We are working with a set of positive and negative integers. If the sum of all the elements in C is strictly lower than T, then there's no way we can sum up to T using the negative elements of C.

We've now ruled out a lot of cases, for every value T. Although this will change in case the value T is positive or negative. If the value T is negative, we just invert the orders of the boolean check and it will comply.

### Step two - Relation between the difference between posSum and negSum, and T.
------------------------------------------------------
This second check will verify that the value T could be achieved, by checking wether the difference between the sum of the positive and negative elements is lower than T.

> If $posSum+negSum \leq T$, then there may be a subset of elements that sum up to T, since we can compensate the positive sum with the negative sum. 

If the condition isn't met, we can't discard the existence of subsets in C that comply with the premise, but we will use this on step three in a particular way.

### Step three - Ruling out useless values.
------------------------------------------------------
Starting with negative ints (since the value T is positive), we check if the lowest value itself can compensate the positive values. We can do this since it's an ordered array from smallest to biggest integer, so we just access the first position on the array. Let the actual lowest value of the array be stored in a variable called lowCheck, and the biggest value in highCheck. We will then do an extra check in case the condition is not met.

> If $posSum+lowCheck \leq T$, then there's no way the positive values can compensate the lowest, so we pop it out of the array as that value is useless for the problem. Here, we are already creating a subset in C itself.

> If $posSum+lowCheck \geq T$, then the value by itself can't compensate the positive values, so we're going to keep it in the set. The lowCheck will now be the next indexed value, and negSum will not consider the value we kept ($negSum = negSum-lowCheck$).

Now that we've checked for the lowest value, and we either ruled it out or kept it in the set, we will go to check the highest value of the array in a similar fashion.

> If $negSum+highCheck \geq T$, then, the inverse to the first negative check happens, and we pop out the highCheck value of the array.

> If $negSum+highCheck \leq T$, such as the second negative check, we'll keep the value on the set, not consider it for the posSum value, and set highCheck to the previous indexed value.

After every positive and negative check, we'll also do an extra check that will be if $posSum+negSum=T$, in case this somehow ends up being true, we've found a subSet that complies. If not, the iterations will continue, which will end up in an O(n) algorithm since every element of the array is only accessed once throughout the whole process.

### Step four - You've found the set (Or not).
------------------------------------------------------
After all these checks, you will be left with two alternatives.

> Either you are left with a subSet of C with two or more elements. Congratulations! You can affirm that the elements of a subset of C sums up to T.

> Or, you're left with an empty array. Congratulations! You can affirm that there isn't any subsets of C which elements sum up to T.