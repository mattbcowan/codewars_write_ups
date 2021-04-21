Your task, is to create NxN multiplication table, of size provided in parameter.

for example, when given `size` is 3:

```
1 2 3
2 4 6
3 6 9

```

for given example, the return value should be: `[[1,2,3],[2,4,6],[3,6,9]]`

---

## Breakdown

Yaaaaay another codewars kata! Alright, in all seriousness let's get to it. I like this problem a lot because it's an issue that you could run into real world and this could actually be useful on it's own. So, this I believe would be called a matrix. Though it is perfectly square I'm not sure if that changes the name at all. Anyways, let's try and figure this out.

So, the inserted number is 3 so we can multiply 3 by itself to make 9. If we do that then we can iterate over the numbers starting with 1 and every 3 items we have in a list we then push the list to a final list and reset the list. Let's give that a shot.

```python
def multiplication_table(size):
    temp_list = []
    table = []
    size_squared = size * size
    
    for i in range(size_squared + 1):
        if len(temp_list) == size:
            table.append(temp_list)
            temp_list = []
            temp_list.append(i + 1)
        else:
            temp_list.append(i + 1)
    return table
```

It works perfect! Except... it doesn't. Why?

Well because I misread the instructions. It's actually suppose to be list 1 is size * 1, list 2 is size * 2 and list 3 is size * 3 all the way up to whatever the final product is. So we have to refactor a bit. Also I wasn't happy with the full implementation of this because of all the +1's. Let's refactor.

```python
def multiplication_table(size):
    temp_list = []
    table = []
    i = 1
    
    while i < size + 1:
        if len(temp_list) == size:
            table.append(temp_list)
            temp_list = []
            i += 1
            temp_list.append(i)
        else:
            temp_list.append(i * (len(temp_list) + 1))
        
    return table
```

Alright, now it works according to the provided test. Here's the breakdown. We make 2 empty lists, 1 for temp items and 1 for the final product. We provide i which equals 1. While i is less than the size provided + 1 (to end the function correctly) then we go through the following steps:

if the length of the temp_list is equal to the size provided then we append the current temp_list to the table, clear the temp_list, add 1 to i and finally add a new item to the current temp_list that is equal to i.

If the length of temp_list isn't equal to the size provided then we append to temp_list i times the length of temp_list + 1.

Why all the +1's? Well the one for size is so that it can go through the entire process of appending the final part of the list to the table. If we didn't have that then the final `if len(temp_list) == size`  would not fire. The second +1 is because we need the next length of the list in order to get an accurate multiplication. We don't want our 2nd number to be multiplied by 1 because the list only has 1 item currently.

Alright, let's run the attempts now and see how we did... and they all passed! Let's take a look at what the number 1 answer is and see how we could have refactored it.

```python
def multiplicationTable(size):
    return [[j*i for j in range(1, size+1)] for i in range(1, size+1)]
```

Cool, so this is a 1 liner that I've been talking about occasionally. While this looks good I feel like I have very little idea what's actually going on. It's awesome that it's 1 line of code but it's hard to read. In my example you know exactly what's going on and how it's functioning even if it's not the most optimized. Let's look at the 2nd top answer:

```python
def multiplication_table(size):
    columns = []
    for i in range(1,size+1):
        rows = []
        for j in range(1,size+1):
            rows.append(i*j)
        columns.append(rows)
        
    return columns
```

This is almost exactly the same as the 1 liner but is much easier to read in my opinion. I feel like I know what is going on here and comprehend what is trying to be achieved. This I could get behind.

## Use Cases

I mentioned matrices (or matrix if singular) near the beginning of the breakdown. This is where this could be extremely useful. When you look into data science and especially machine learning matrix multiplication is almost always in use. This type of equation would be perfect for generating some data to play with and see how your program is working if you were wanting to do it from scratch. This is where I feel this program would shine.

## Wrapping Up and Final Thoughts

About a year or 2 ago I feel this problem would have given me a lot of trouble. It's extremely good for beginners because it makes you think outside of the box a little. There's small issues that you have to resolve in order to get your program up and running fully and I really like that. If it was just generating a list of numbers from the given size squared then it would have been far too easy (as you saw with me solving that first without much thought) but the addition of multiplying by the row you're on helped flesh out this problem and make it much more challenging.