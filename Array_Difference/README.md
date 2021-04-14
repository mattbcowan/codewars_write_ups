Your goal in this kata is to implement a difference function, which subtracts one list from another and returns the result.

It should remove all values from list a, which are present in list b.

```python
array_diff([1,2],[1]) == [2]
```

If a value is present in b, all of its occurrences must be removed from the other:

```python
array_diff([1,2,2,2,3],[2]) == [1,3]
```

---

## Breakdown

So, as with all code there's 1,000+ different ways to write this. The easiest way I've found is with a simple 1 liner for the code:

```python
def array_diff(a, b):
	newList = [i for i in a if i not in b]
	return newList
```

This code creates a new list called newList and for the current list item in a if it's NOT in b then it's OK to add to newList. If it is in b then it's skipped.

After it has iterated though all of the items in list a and checked if it exists in list b then it returns newList.

That's it!

## Use Cases

The hardest thing for me to be able to comprehend currently is when we would even use this in a real world scenario. To preface this, I'm not a full time programmer or software engineer. I code for fun on the side and enjoy learning how computers work. That's it. That being said, when would we use this?

For this specific set of code I'd think of it more as a clean up scenario. We have a list of Netflix shows we think a customer might like based on previous shows they have watched. We pull together a list of shows that fit the criteria and then compare it to their already watched shows. If they have already watched it then we don't want to recommend it again (For this example. I know Netflix has a "Watch it again" section). So we have a list like so:

```python
recommendations = ["The Office", "Parks and Recreation", "Community"]
watchedShows = ["The Office"]
```

Now we will run it through our function:

```python
array_diff(recommendation, watchedShows)
```

and the results are what you would expect:

```python
["Parks and Recreation", "Community"]
```

This is one of the many reasons I think this program would be used. I'm sure there's many more and feel free to tell me if I'm wrong.