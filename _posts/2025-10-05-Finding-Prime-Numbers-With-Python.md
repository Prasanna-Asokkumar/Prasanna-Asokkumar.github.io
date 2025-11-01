---
layout: post
title: Finding Prime Numbers with Python
image: "/posts/Prime_Numbers_Project_Image.webp"
tags: [Python, Primes]
---

In this post, I'm going to run through a function in Python that can quickly find all the prime numbers below any given value. For example, if I passed the function a value of 100, it would find all the prime numbers below 100!

A prime number is a number that can only be divided wholly by itself (e.g. 11 is a prime number as no other numbers apart from 11 or 1 divide cleanly into it, but 24 is not a prime number as while 24 and 1 divide into it, so do 2, 3, 4, 6 and 8.

---

First let's start by setting up a variable that will act as the upper limit of numbers that we want to search through for prime numbers within that range. We'll start with 30, so we're essentially wanting to find all prime numbers that exist that are equal to or smaller than 30.

```ruby
n = 30
```

The smallest true Prime number is 2, so we want to start by creating a list of numbers that need checking so that every integer between 2 and what we set above as the upper bound which in this case was 30. We use n+1 as the range logic is not inclusive of the upper limit we set there.

Instead of using a list, we're going to use a set. This is because sets have functionality that will allow us to eliminate non-primes during our search.

```ruby
number_range = set(range(2, n + 1))
```

To store any primes that we get back, we can create a place to store it in. A list is what can be used for this.

```ruby
primes_list = []
```

We're going to end up using a while loop to iterate through our list and check for primes, but before we do that, I will code up the logic first and iterate it manually. This means that I can check that the code is working correctly before I set it off to run through everything on it's own.

So, we have our set of numbers (called number_range) to check all integers between 2 and 30. Let's extract the first number from that set that we want to check as to whether it's a prime. When we check the value we're going to check if it is a prime. If it is, we're going to add it to our list called primes_list. If it isn't a prime, we don't want to keep it.

There is a method which will remove an element from a list or set and provide that value to us, and that method is called *pop*.

```ruby
print(number_range)
>>> {2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29, 30}
```
If we use pop, and assign this to the object called **prime** it will *'pop'* the first element from the set out of **number_range**, and into **prime**

```ruby
prime = number_range.pop()
print(prime)
>>> 2
print(number_range)
>>> {3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29, 30}
```

Now, we know that the very first value in our range is going to be a prime number becauses there is nothing smaller than it (the number 2). Therefore, nothing else could possibly divide evenly into it. As we know it's a prime, we can add it to our list of primes (primes_list).

```ruby
primes_list.append(prime)
print(primes_list)
>>> [2]
```

Now it is time to check our remaining number_range for non-primes following the number 2 being dropped from this set. For the prime number we just checked (in this first case it was the number 2) we want to generate all the multiples of that up to our upper range (in our case, 30).

We're going to again use a set, called **multiples**, rather than a list, because it allows us some special functionality that we'll use soon.

```ruby
multiples = set(range(prime * 2, n + 1, prime))
```

The syntax for creating a range is: range(start, stop, step). For the starting point - we don't need our number as that has already been added as a prime, so let's start our range of multiples at 2 * our number as that is the first multiple. In this case, our number is 2 and so the first multiple will be 4. If the number we were checking was 3 then the first multiple would be 6 - and so on.

For the stopping point of our range - we specify that we want our range to go up to 30, so we use n + 1 to specify that we want 30 to be included.

Now, the **step** is key here.  We want multiples of our number, so we want to increment in steps *of our* number - so we can put in **prime** here as the step.

Lets have a look at our list of multiples:

```ruby
print(multiples)
>>> {4, 6, 8, 10, 12, 14, 16, 18, 20, 22, 24, 26, 28, 30}
```

The next part is using the special set functionality **difference_update**, which removes any values from our number range that are multiples of the number we just checked. The reason we're doing this is because if a number is a multiple of anything other than 1 or itself then it is **not a prime number** and we can therefore remove it from the list to be checked.

Before we apply the **difference_update**, let's look at our two sets.

```ruby
print(number_range)
>>> {3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29, 30}

print(multiples)
>>> {4, 6, 8, 10, 12, 14, 16, 18, 20, 22, 24, 26, 28, 30}
```

**difference_update** works in a way that will update the first set (in this case, **number_range**) to only include the values that are *different* from those in a second set (in this case, **multiples**).

To use this, we put our initial set and then apply the difference update with our multiples

```ruby
number_range.difference_update(multiples)
print(number_range)
>>> {3, 5, 7, 9, 11, 13, 15, 17, 19, 21, 23, 25, 27, 29}
```

When we look at our number range now, all values that were also present in the multiples set have been removed as we *know* they were not primes.

We'v now made a massive reduction to the pool of numbers that need to be tested, so this is really efficient. It also means the smallest number in our range *is a prime number* as we know nothing smaller than it divides into it. So this means we can run all that logic again from the top!

Whenever you can run sometime over and over again, a while loop is often a good solution.

Here is the code, with a while loop, doing the work of updated the number list and extracting primes until the list is empty. Let's run it for any primes below 1000.

```ruby
n = 1000

# number range to be checked
number_range = set(range(2, n + 1))

# empty list to append discovered primes to
primes_list = []

# iterate until list is empty
while number_range:
    prime = number_range.pop()
    primes_list.append(prime)
    multiples = set(range(prime * 2, n + 1, prime))
    number_range.difference_update(multiples)
```

Let's print the primes_list to have a look at what we found.

```ruby
print(primes_list)
>>> [2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47, 53, 59, 61, 67, 71, 73, 79, 83, 89, 97, 101, 103, 107, 109, 113, 127, 131, 137, 139, 149, 151, 157, 163, 167, 173, 179, 181, 191, 193, 197, 199, 211, 223, 227, 229, 233, 239, 241, 251, 257, 263, 269, 271, 277, 281, 283, 293, 307, 311, 313, 317, 331, 337, 347, 349, 353, 359, 367, 373, 379, 383, 389, 397, 401, 409, 419, 421, 431, 433, 439, 443, 449, 457, 461, 463, 467, 479, 487, 491, 499, 503, 509, 521, 523, 541, 547, 557, 563, 569, 571, 577, 587, 593, 599, 601, 607, 613, 617, 619, 631, 641, 643, 647, 653, 659, 661, 673, 677, 683, 691, 701, 709, 719, 727, 733, 739, 743, 751, 757, 761, 769, 773, 787, 797, 809, 811, 821, 823, 827, 829, 839, 853, 857, 859, 863, 877, 881, 883, 887, 907, 911, 919, 929, 937, 941, 947, 953, 967, 971, 977, 983, 991, 997]
```

Let's now get some interesting stats from our list which we can use to summarise our findings - the number of primes that were found and the largest prime in the list.

```ruby
prime_count = len(primes_list)
largest_prime = max(primes_list)
print(f"There are {prime_count} prime numbers between 1 and {n}, the largest of which is {largest_prime}")
>>> There are 168 prime numbers between 1 and 1000, the largest of which is 997
```

The next thing to do would be to put it into a function, which you can see below:

```ruby
def primes_finder(n):
    
    # number range to be checked
    number_range = set(range(2, n + 1))

    # empty list to append discovered primes to
    primes_list = []

    # iterate until list is empty
    while number_range:
        prime = number_range.pop()
        primes_list.append(prime)
        multiples = set(range(prime * 2, n + 1, prime))
        number_range.difference_update(multiples)
        
    prime_count = len(primes_list)
    largest_prime = max(primes_list)
    print(f"There are {prime_count} prime numbers between 1 and {n}, the largest of which is {largest_prime}")

    return primes_list
```

Now we can just pass the function with a parameter (a number in this case that represents the upper bound of our search, e.g. 5000 if we want to find the list of prime numbers up to 5000) and it will do the rest!

Let's go for something large, say a million...

```ruby
primes_finder(1000000)
>>> There are 78498 prime numbers between 1 and 1000000, the largest of which is 999983
```
---

###### Important Note: Using pop() on a Set in Python

In the real world - we would need to make a consideration around the pop() method when used on a set as in some cases it can be a bit inconsistent.

The pop() method will usually extract the lowest element of a set. Sets however are, by definition, unordered. The items are stored internally with some order, but this internal order is determined by the hash code of the key (which is what allows retrieval to be so fast). 

This hashing method means that we can't 100% rely on it successfully getting the lowest value. In very rare cases, the hash provides a value that is not the lowest.

The simplest solution to force the minimum value to be used is to replace the line...

```ruby
prime = number_range.pop()
```

...with the lines...

```ruby
prime = min(sorted(number_range))
number_range.remove(prime)
```

...where we firstly force the identification of the lowest number in the number_range into our prime variable, and following that we remove it.

However, because we have to sort the list for each iteration of the loop in order to get the minimum value, it's slightly slower than what we saw with pop().

Therefore, the final code for the **prime_finder** function, for good measure, is:

```ruby
def primes_finder(n):
    number_range = set(range(2, n + 1))          
    primes_list = []                            

    while number_range:
        prime = min(sorted(number_range))
        number_range.remove(prime)
        primes_list.append(prime)
        multiples = set(range(prime * 2, n + 1, prime))
        number_range.difference_update(multiples)

    print(primes_list)

    prime_count = len(primes_list)

    largest_prime = max(primes_list)

    print(f"There are {prime_count} prime numbers between 2 and {n}, the largest of which is {largest_prime}")
    
    return primes_list
```
And to call the function, we use the input parameter as the upper bound of the range we want to look at (so for below, it is prime numbers up to a 100):

```ruby
primes_list =  primes_finder(100)
>>> [2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47, 53, 59, 61, 67, 71, 73, 79, 83, 89, 97]
There are 25 prime numbers between 2 and 100, the largest of which is 97
```
