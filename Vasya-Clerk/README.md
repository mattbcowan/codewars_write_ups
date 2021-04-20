The new "Avengers" movie has just been released! There are a lot of 
people at the cinema box office standing in a huge line. Each of them 
has a single `100`, `50` or `25` dollar bill. An "Avengers" ticket costs `25 dollars`.

Vasya is currently working as a clerk. He wants to sell a ticket to every single person in this line.

Can Vasya sell a ticket to every person and give change if he 
initially has no money and sells the tickets strictly in the order 
people queue?

Return `YES`, if Vasya can sell a ticket to every person and give change with the bills he has at hand at that moment. Otherwise return `NO`.

### Examples:

```python
tickets([25, 25, 50]) # => YES 
tickets([25, 100]) # => NO. Vasya will not have enough money to give change to 100 dollars
tickets([25, 25, 50, 50, 100]) # => NO. Vasya will not have the right bills to give 75 dollars of change (you can't make two bills of 25 from one of 50)
```

## Breakdown

OK, so this one seems pretty straight forward. Let's visualize a few options that will work.

```python
[25, 25, 50]
50 / 25 = 2
YES

[25, 25, 25, 100]
100 / 25 = 4
YES
```

So, if we have 3 25's out of 4 items we can do it. So let's think about it like this...

If we have 10 people in line how many need to spend 25 for us to have change?

```python
[25, 25, 25, 25, 25, 25, 25, 25, 25, 25]
YES

[25, 25, 25, 25, 25, 25, 25, 25, 25, 50]
YES

[25, 25, 25, 25, 25, 25, 25, 25, 50, 50]
YES

...

[25, 25, 25, 25, 50, 50, 50, 50, 50, 50]
NO
```

So, for every 50 we need 1 25. For every 100 we need 3.

```python
[25, 25, 50, 100]
count_of_25s = 2
25s_needed = 4

for every 50 -> 25s_needed += 1
for every 100 -> 25s_needed += 3
for every 25 -> count_of_25s += 1
Compare.
```

I think this concept will work. Let's give it a try. 

```python
def tickets(people):
    # Adds the number of 50's change and 100's change together
    change_needed = people.count(50) + people.count(100) * 3
    # Check if change needed is less than or equal to the number of 25's 
    if change_needed <= people.count(25):
        return "YES"
    else:
        return "NO"
```

We are close. So we can also offer a 50 and a 25 for the 100. This just got a bit more complicated. Let's get it working and then we can work backwards and figure out the best way to solve:

```python
def tickets(people):
    count_of_25 = 0
    count_of_50 = 0
    
    for i in people:
        if i == 100 and count_of_50 > 0 and count_of_25 > 0:
            count_of_25 -= 1
            count_of_50 -= 1
        elif i == 100 and count_of_25 >= 3:
            count_of_25 -= 3
        elif i == 50 and count_of_25 > 0:
            count_of_25 -= 1
            count_of_50 += 1
        elif i == 25:
            count_of_25 += 1
        else:
            return "NO"
    return "YES"
```

Alright, so we have it passing all tests. Now to figure out how awful my current code is and refactor. So the issue is we don't know what the next payment will be so we have to go in order, however this might allow us to refactor and make it much simpler code. 

Let's fast forward a bit. I've been looking at this code for about 30 minutes now and I'm not seeing a better way to refactor it. If we add the change without keeping track of the 50's and 25's then we will not be able to figure out if we actually have the bills for correct change. Because of that I feel like we are at a good place. Let's find out what the #1 answer is.

```python
def tickets(people):
  till = {100.0:0, 50.0:0, 25.0:0}

  for paid in people:
    till[paid] += 1
    change = paid-25.0
    
    for bill in (50,25):
      while (bill <= change and till[bill] > 0):
        till[bill] -= 1
        change -= bill

    if change != 0:
      return 'NO'
        
  return 'YES'
```

Cool. Let's take a look at this and break it down. 

Instead of having 2 different variables they just used a dictionary. That's a lot smarter. After that they send whatever was paid (aka the current number in the list) to the dictionary then they determine the change based on what was paid -25.

After that they cycle through the bills 50 and 25. While the bill is less than the change and the bill chosen is greater than 0 then they can remove the bill and deduct that from the change needed. This means that if the change is under 50 then it will choose 25 and if there are no 50's for a 100 then it will use all 25s.

If the change doesn't hit 0 then it fails and says "NO" but if it succeeds through the entire process it returns "YES".

Nice! This is way more complicated than I thought at first. It really tested my thinking. I'm going to try and remember a few of these tricks in the future and also start using dictionaries where they are best used.

## Use Cases

I feel like this actually has a lot of use cases. Using dictionaries to keep things accounted for like they did in the #1 answer is extremely useful. As a specific use case outside of money I can't think of one off the top of my head but going through lists given and keeping track of data is useful in a ton of data science scenarios. 

## Wrapping Up and Final Thoughts

Unfortunately my brain is fried due to this one. I know it's a simple one but trying to think through the entire problem while not looking much up on google (trying to minimize finding an answer by accident while I'm working through it) really burns me out. This was an extremely interesting problem to figure out. That's all I've got.