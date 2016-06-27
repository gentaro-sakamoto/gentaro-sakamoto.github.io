---
published: true
title: `[WIP`] Code Dojo `#1. Can You Convert Number To String?
layout: post
---
### Design the program before implementing

To begin with, let's think about what type of numbers are there in the world to design the program.

#### Type of numbers
- negative and positive integer
- zero
- floating point
- one..ten, eleven, twenty, thirty, hundred
- thousand, million, billion, trillion etc

Next, how do we handle the largest and the smallest number? Let's think about possible ways.

#### Ideas
- Set the limit
- Allow to work with the infinite parameter but how? It sounds hard though.

Next, how do we handle the unexpected parameters? What type of it could be passed?

#### Unexpected parameters
- Multibyte string such as 'あ'
- Not a number such as 'hoge'
- Strings which contains comma such as '1,000,000'
- Strings which contains whitespace such as '1 000 000'
- Strings which contains both string and number such as '1000円'

We could list the possible unexpected parameters like the above. This time, if the application receive just a string or the number containing string, then let's raise exception with messages which tell the user what happend. And, if the application receive a number containing whitespaces or commas, let's remove them and convert the valid number to the word. That's our validation specification.

### Do the coding
TBD

#### Write test first and make sure it fails
TBD

#### Implement to pass the tests
TBD

### Performance tuning
- bench mark

TBD

### Sample Code
[gentaro-sakamoto/number2word](https://github.com/gentaro-sakamoto/number2word)


### References
- [radar/humanize](https://github.com/radar/humanize)
