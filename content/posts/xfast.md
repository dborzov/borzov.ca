+++
date = "2013-12-01T08:36:54-07:00"
draft = true
title = "Predecessor search for Big Data: x-fast tries, locality of reference and all that"

+++


Predecessor search for Big Data: x-fast tries, locality of reference and all that
==============================

### *Abstract:* An intro to data structures with locality of reference-type features. We review x-fast tries in somewhat greater detail and then talk some about similar data structures, where they shine and where they don't.


Let's consider a set `{n}` of `n` integers within the range `0..M`. We want to build a data structure that allows for performing the following operations efficiently:

* **Look up**: check if an integer `i` is in ``{n}``
* **Update**: Insert or delete an integer into `{n}`.

This is how commonly used data structures are holding up with these: or simply a sorted array, balanced binary search tree (whether it is AVL or red-black), hash table:

 | Lookup | Update
--- | --- | ---
Sorted Array | Ө(log(n)) | O(n)
Balanced binary tree | Ө(log(n)) | Ө(log(n))
Hash Table | Ө(1) | Ө(1)

The design of hash table is tailored for these two operations, however it performs really badly when we add the following operation:

* **Find predeccessor item**: look up an integer `i` and if it is not in the set, return the closest int within the set that is lower by value  (i.e, for 4 in {1,5,7} that would be 1)

Here is how we can implement Predecessor Search for these data structures:
* **Sorted Array** we search for the value using binary search and when if turns out that it is not within the set, we simply take the next left value from where it should have been.
* **Binary Tree** is similar to **Sorted Array**: we traverse to the next item left from where the value should have been
* **Hash Table**: if the value `i` is not within the set, the only option is to look up `i-1` and continue looking up lower values until we stumble upon one within the set. The average distance between the two values is asymptotically `Ө(M/n)`

| Lookup | Update | Predeccessor
--- | --- | ---
Sorted Array | Ө(log(n)) | O(n) | O(log(n))
Balanced binary tree | Ө(log(n)) | Ө(log(n)) | O(log(n))
Hash Table | Ө(1) | Ө(1) | O(M/n)

We see that the advantage of hash table evaporates before the predecessor operation. Is there a way to improve the data structure to fix this bad performance?

One way to improve things that may come to mind at this point is to just store pointers to predecessor and successor elements for each of the the elements within `M` right in the hash table.

This would enable quick predecessor lookup, but would break the performance of the Update function.

Indeed, adding a value for this Hash-Table with Predecessor Pointers means we need to update the predecessor pointer for all values from the new one to the closest larger one. Which, again, means O(M/n) operations.

There is another drawback to this approach. Now we have to store not just `n` set elements but all the `M` possible values. Plus, each of the pointers is bound by `O(log(M))` needed to store the value position. So the total size of the structure depends on M, which gets quite large.

So we sum up the state of affairs with this table:

| Lookup | Update | Predeccessor | Size
--- | --- | ---
Sorted Array | Ө(log(n)) | O(n) | O(log(n)) | Ө(n)
Balanced binary tree | Ө(log(n)) | Ө(log(n)) | O(log(n)) | Ө(n)
Hash Table | Ө(1) | Ө(1) | Ө(M/n) | Ө(n)
Hash-Table with Predecessor Pointers | Ө(1) | Ө(1) | Ө(M/n) | Ө(M log(M))

### Reflection
Let's reflect on these results a little bit. We see that hash table is ideal when we look up exact values but breaks down when we start inquiring on operations where *locality* of the value starts to matter.

That makes sense. A proper hash function means a mathematically unstable one: even small increments change the hash completely which insures the values are spread homogeneously among the hash table buckets.

At the same time we see that binary search tree-style data structures are quite resielent to *locality* context class of problem. The search tree is based on the concept of proximity and contains the information about locality.

X-fast trie is a data structure that combines the advantages of both search trees and hash tables.

### Enter X-fast trie
Let's build a bitwise search tree on top of all the M elements. All the M values are at the tree's leaves (which makes it a )


### Back to Earth (practical examples)
Imagine you are developing a flight search website. You need to build a tailored search index that is really good at one task: for the given date and search criteria returns the list of the next flights. How is it better to approach designing this?

This is an example of encountering predecessor/successor search problem in practice. Predecessor/successor search (or predecessor search for 'short') stands for cases when we have some dictionary (*in our case, the flight timetable*) with its keys (*departure times*) making up a subset of some ordered sequence (*like all minutes in the time range of interest or something*).

We want to design a system that for a given query element of that sequence (such as *our preferable flying date and time*) would return the closest next subsequest element that is a key of our dictionary(*next flight from our query time*):

\[
![Img](./img/arrow.png)
\]





For the flight search website that would include the two following typical database tasks:


This problem of optimizing the flight is an example of the predecessor/successor search problem. It involves storing a dictionary with keys that make a subset of some subset

 X-fast tries is a data structure designed to store a subset of elements from a sequence in such a way so that it is possible to retrieve the next closest  element easily. People refer to such problems as the predecessor/ successor search.

I have been reading up about [x-fast tries](http://en.wikipedia.org/wiki/X-fast_trie) recently and decided to write up some notes  and also to make a simple python implementation to go along with it.


Obviously, it is incredibly inefficient to just store them as a list and go through the list each time. We need some kind of a search tree.

Times of planes leaving (for example, in minutes) make up a subset of all the possible times (all minutes in the span of the existence of that specific airport). thus we can store them as a set of natural numbers  in a certain range of integers.

So with that representation we can use the Direct Access Table. That is, make up a binary table of size  |S|.

What are some ways to tackle the predecessor/successor problem? Naively we can just move along up the Direct Access Table until we stumble upon the next record. The worst case scenario here would be just going through the whole empty table, **O(m)** .

One improvement is to store pointers for predecessor/successor elements at each element. It does improve the predecessor/successor retrieval to just **O(1)** . However, we have a inserting or deleting elements becomes a costly procedure as we have to reassign every nearby value, **O(m)**.

Here is the proposition. We build an ordered binary tree atop the m elements and mark every node that is a parent of the checked element. With each layer having two times less elements we only increased the total size of the array by two.

## Toy problem
Here is a nice toy problem that I came up with in hope that it would make some cool-loking plots.

With movie and tv show titles such as X-files, X-men and American History X, we have a trope of X standing for anything misterious (and heavily implied to be hip). How often are movies with such titles released? What was the first X-movie released since your birth?

To find out, let us build the X-fast trie index for such X-titled movies by the year of release.

Here is a plot of what we get:

\[
![Img](./img/movies.png)
\]

Pretty nice, huh?
