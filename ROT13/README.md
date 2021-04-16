ROT13 is a simple letter substitution cipher that replaces a letter with the letter 13 letters after it in the alphabet. ROT13 is an example of the Caesar cipher.

Create a function that takes a string and returns the string ciphered with Rot13. If there are numbers or special characters included in the string, they should be returned as they are. Only letters from the latin/english alphabet should be shifted, like in the original Rot13 "implementation".

Please note that using `encode` is considered cheating.

---

## Breakdown

To preface this, I'm writing down things as I think about it. This is how I think about things when I'm coding and the process I go through. I find it more helpful to myself when I look back so I'm giving this format a test run.

OK, so... we need to do a little planing on this one. It doesn't look like it will be that bad but lets lay out the steps..

Step 1: Iterate over the characters in the string.

Step 1.1: Check if character is a letter

Step 1.2: If it is a letter get the ASCII value of the character and add 13 to the value. Add value to a list.

Step 1.3: If it is not a letter, add character to a list

Step 2: Concatenate list

Step 3: Return

In theory this works... except that if we get a letter past "N" or "n" then we go off the ASCII value chart for letters. So we need to add in an exception that if it is "n" or "N"s value subtract 13 instead. so that's fairly straight forward:

```python
def rot13(message):
	for i in message:
		if i.isdigit():
			# Add the digit to list
		else:
			if ord(i) >= ord("n") or ord(i) >= ord("N"):
				# Subtract from ASCII value
			else:
				# Add to ASCII value
```

This, however, still won't work. The reason being that any letter higher than the value of "N" will set this off. Unfortunately that includes any lowercase letters.

Reference: [sticksandstones.kstrom.com/appen.html](http://sticksandstones.kstrom.com/appen.html)

So instead I'm going to go with lowercase for all letters for checking the value.

```python
def rot13(message):
	for i in message:
		if i.isdigit():
			# Add the digit to list
		else:
			if ord(i.lower()) >= ord("n"):
				# Subtract from ASCII value
			else:
				# Add to ASCII value
```

Now we have something that theoretically should get us in the right place. Let's add in a list to add the letters to and add those letters in and then join it all together and return the final product.

```python
def rot13(message):
	cipher = []
	for i in message:
		if i.isdigit():
			cipher.append(i)
		else:
			if ord(i.lower()) >= ord("n"):
				cipher.append(chr(ord(i) - 13))
			else:
				cipher.append(chr(ord(i) + 13))
	return ''.join(cipher)
```

We are almost there. We have one issue, not all special characters are numbers. The isdigit() is only looking for numbers so we need anything that isn't a letter.

```python
def rot13(message):
	cipher = []
	for i in message:
		if i.isalpha() is False:
			cipher.append(i)
		else:
			if ord(i.lower()) >= ord("n"):
				cipher.append(chr(ord(i) - 13))
			else:
				cipher.append(chr(ord(i) + 13))
	return ''.join(cipher)
```

Nice. The tests have run and come back positive. Now let's see if there's anything we can do to improve this. There looks to be a lot of if/else statements so can we reduce these? Let's try throwing in a ternary operator.

```python
def rot13(message):
    cipher = []
    for i in message:
        if i.isalpha() is False:
            cipher.append(i)
        else:
            cipher.append(chr(ord(i) - 13) if ord(i.lower()) >= ord("n") else chr(ord(i) + 13)) 
    return ''.join(cipher)
```

Alright that got rid of 1 of the cipher.append's. We can make it a little more legible if we label the chr(ord()) statements so let's do that so we can see this a bit more clear. Let's also make it a bit more clear what we are doing in the for loop because i isn't very descriptive.

```python
def rot13(message):
    cipher = []
    for character in message:
        subtract_13 = chr(ord(character) - 13)
        add_13 = chr(ord(character) + 13)
        
        if character.isalpha() is False:
            cipher.append(character)
        else:
            cipher.append(subtract_13 if ord(character.lower()) >= ord("n") else add_13) 
            
    return ''.join(cipher)
```

Nice! Now we can read it a bit better. I'm pretty happy with this. I'm sure there's a 1 liner that looks absolutely insane and is 1000% faster but I can read this and understand what's going on so I'm going to say that's good enough.

## Use Cases

Rot13 is as it says in the instructions. It's a Caesar cipher. This is an old way of encrypting data and isn't used too much to my knowledge since we have much better ways of encrypting now. Still, it's a base level understanding of how cipher's work. You have to understand the fundamentals before you move on to the high level understanding. Lay a strong foundation for the house then build the frame.

## Wrapping Up and Final Thoughts

This was a lot of fun to work through. There were a few hiccups but overall this went well. I feel like I'm fleshing out my knowledge of code better as I do these write ups and I would like to do them daily. We will see how long it lasts.