Given: an array containing hashes of names

Return: a string formatted as a list of names separated by commas 
except for the last two names, which should be separated by an 
ampersand.

Example:

```python
namelist([ {'name': 'Bart'}, {'name': 'Lisa'}, {'name': 'Maggie'} ])
# returns 'Bart, Lisa & Maggie'

namelist([ {'name': 'Bart'}, {'name': 'Lisa'} ])
# returns 'Bart & Lisa'

namelist([ {'name': 'Bart'} ])
# returns 'Bart'

namelist([])
# returns ''
```

---

## Breakdown

Awesome, this one doesn't look too complicated. Here's what I'm thinking we need to do:

1. Make an empty string
2. Get length of the list of names.
3. Iterate over the list
    1. If the list is 1 or 0 or the current list item is the last one then we append what we have with no changes
    2. If the current name we are on is 2 less than the length of the list then we add an ampersand and append to the string
    3. Else add a comma and append to the string.
4. Return the string.

Alright, with that out of the way let's start implementing it. I haven't worked with dictionaries too much in python so let's figure out how to get the values in the dictionary. Since I said get the values let's try using get:

```python
def namelist(names):
	for i in names:
		print(i.get('name'))
```

Aaaaand it works. Perfect. Now let's try and figure out the length of the list and start implementing our pseudo code.

```python
def namelist(names):
    list_length = len(names)
    final_string = ''
```

We have our length of the list and our final string ready

```python
def namelist(names):
    list_length = len(names)
    final_string = ''
    for i in names:
        if list_length <= 1 or names.index(i) == list_length - 1:
            final_string += i.get('name')
```

We now have the first piece of iteration working. This will pass if the string is 0 or 1 in length.

```python
def namelist(names):
    list_length = len(names)
    final_string = ''
    for i in names:
        if list_length <= 1 or names.index(i) == list_length - 1:
            final_string += i.get('name')
        elif names.index(i) == list_length - 2:
            final_string = final_string + i.get('name') + " & "
        else:
            final_string = final_string + i.get('name') + ", "
    return final_string
```

Alright now we added in if the index is 2 before the end then add in an & else add in a comma. Our test cases are passing so let's see if it fully passes aaaand... It does. So let's do some clean up to make it more legible.

```python
def namelist(names):
    list_length = len(names)
    final_string = ''
    for value in names:
        if list_length <= 1 or names.index(value) == list_length - 1:
            final_string += value.get('name')
        elif names.index(value) == list_length - 2:
            final_string = final_string + value.get('name') + " & "
        else:
            final_string = final_string + value.get('name') + ", "
    return final_string
```

Not much clean up today. All I did was replace i with value since it's a little more descriptive. Overall this one was pretty easy to figure out. I thought about doing a switch case statement but I feel that there isn't enough here to really justify it. 

Looking at the other user submitted code after submitting mine it looks like people are using .format to make the join statement a bit easier to read and probably a little faster so we could have done that. I'll try and remember that next time we are formatting strings.

## Use Cases

For this one I think it's more about the actual knowledge of how to work with dictionaries and formatting strings that's useful. The actual need for formatting a string with commas and an ampersand may come into play in a front end application but other than that I can't really say where I would see this particular function being used.

## Wrapping Up and Final Thoughts

All in all this is a good kata for basic knowledge (which explains why it's in the fundamentals section). I'll definitely try to remember the .format when using strings next time as that will make the code much cleaner. I'd like to start looking into better ways of implementing stuff outside of if/else statements since they can start to take a long time when you have a bunch of them.