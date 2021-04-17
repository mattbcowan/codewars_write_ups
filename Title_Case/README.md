A string is considered to be in title case if each word in the string is either (a) capitalised (that is, only the first letter of the word is in upper case) or (b) considered to be an exception and put entirely into lower case unless it is the first word, which is always capitalised.

Write a function that will convert a string into title case, given an optional list of exceptions (minor words).  The list of minor words will be given as a string with each word separated by a space.  Your function should ignore the case of the minor words string -- it should behave in the same way even if the case of the minor word string is changed.

###Arguments (Haskell)

- **First argument**: space-delimited list of minor words that must always be lowercase except for the first word in the string.
- **Second argument**: the original string to be converted.

###Arguments (Other languages)

- **First argument (required)**: the original string to be converted.
- **Second argument (optional)**: space-delimited list of minor words that must always be lowercase except for the first word in the string. The JavaScript/CoffeeScript tests will pass `undefined` when this argument is unused.

###Example

```python
title_case('a clash of KINGS', 'a an the of') # should return: 'A Clash of Kings'
title_case('THE WIND IN THE WILLOWS', 'The In') # should return: 'The Wind in the Willows'
title_case('the quick brown fox') # should return: 'The Quick Brown Fox'
```

## Breakdown

As is tradition as of the last kata I will be writing this as I go and trying to explain my thought process along the way. I also understand there are probably much better solutions but my main focus is legibility first then overall speed and data use. I feel legibility will help right now and speed and data will take a focus soon after.

Alright so first things first we should probably make sure everything is lowercase to start. We don't want any stray uppercase letters that will throw off the program.

```python
def title_case(title, minor_words=''):
    title = title.lower()
    minor_words = minor_words.lower()
```

Perfect. Now we can start worrying about logic. I think the best way to handle this is to split up the strings into individual words within a list. One list for each input.

```python
def title_case(title, minor_words=''):
    title = title.lower().split(' ')
    minor_words = minor_words.lower().split(' ')
```

Now we can iterate over them and see if any of the words in the minor words are located in the title, but first let's take care of the items we know will be uppercase. So we know the first word and last word will always be uppercase.

```python
def title_case(title, minor_words=''):
    title = title.lower().split(' ')
    minor_words = minor_words.lower().split(' ')
    final_title = []
    for word in title:
        if title.index(word) == 0 or title.index(word) == len(title) - 1:
            final_title.append(word.capitalize())
    print(final_title)
```

While this works for the most part there is one issue. In the 3rd test "The" is put into the final title twice. This is because "The" is written twice in the title and both times its the "first word" because it matches the spelling of the first word. This isn't going to work so we need a different approach.

```python
def title_case(title, minor_words=''):
    title = title.lower().split(' ')
    minor_words = minor_words.lower().split(' ')
    final_title = []
    for word in title:
        if len(final_title) == 0 or title.index(word) == len(title) - 1:
            final_title.append(word.capitalize())
    print(final_title)
```

There we go. No need to match the first word since we will know for sure that it's the first word. How about we go ahead and do that with the last word as well just in case. We can do that by removing words as we go through the original list and adjust them.

```python
def title_case(title, minor_words=''):
    title = title.lower().split(' ')
    minor_words = minor_words.lower().split(' ')
    final_title = []
    for word in title:
        if len(final_title) == 0 or len(final_title) == len(title) - 1:
            final_title.append(word.capitalize())
        else:
            final_title.append(word)
    print(final_title)
```

Perfect. Now we just have to worry about the middle words. That should be pretty simple. If they aren't in the list of minor words then they should be capitalized.