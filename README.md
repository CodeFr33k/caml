# CAML

## Table of Contents

1. [Motivation](#motivation)
1. [Structure](#structure)
1. [Examples](#examples)

## Motivation

Content is king and context is his right hand man.  
Whether it is writing in a scrapbook,  
or typing in a text file,  
there is usually more than just content one wishes to record.  
Many times I write something down,  
I want to know where I got that information, who said it, when did I write this down, etc.   
In the technical writing world citation and referencing styles exist;  
but these usually split content from context,  
leaving the typical writer with content,  
and an incomplete and meaningless mess at the end of a file.  
Of course there is a learning curve to using the citation and referencing features of document editors like Microsoft Word.  
All of this adds friction to the typical writer who does not care about formatting or printing or publishing peer reviewed work.  
Most people are not writing essays in their spare time.  
There are various markup languages out there,  
Markdown, YAML, and TOML are some; each has it's usecases, but not fit for purpose.  
Git also implements tracking behaviour, or context,  
but once again, the separation and complexity of operating more software to achieve something that could be done inline is overkill for the typical person.  
The following language specification outlines a way of combining content and data and also supports the usecase of event sourcing as it is a related activity.

## Structure

### Simple
```
content
content (`data)
@decorator content
@decorator content (`data) 
@decorator (`data) 
```
### Simple Numbering
```
[1] (`data)

@decorator content [1]
```
### Complex 
```
content1
(`
  tag1
  tag2
  key1 = value1

  [group1]
  key2 = value2

  [group2]
  key3 = value3
)
@decorator1 content2 (`key4 = value4)
@decorator2 content3
@decorator3 (`key5 = value5)

# comment
content4 (`key4 = value4)
```
### Numbering System
```
# data declared with id
[1] @decorator1 (`key = value)
[2] (`key = value)

# content then references data by id
content1 [1]
@decorator2 content2 [2]

# override id 1's data
[1] (`
  key1 = value1
  key2 = value2
)

# can place reference at front of content as well
[1] content3
```
### Rules
- any record must contain content or data
- tags, keys, groups, and decorators are _case insensitive_
- decorator names are _hyphen separated_
- data cannot be inlined unless data contains single key-value pair or single tag
- nested groups not supported to promote simple syntax
- tags inside of groups not supported

### Notes on syntax 

The idea of adding extra informative data AFTER content is inspired by the way Python docstrings are formatted, but instead of triple quotes, parantheses and a backtick are used.  
The rest of the syntax is mostly adapted from other common languages.  

## Examples 

### Verbose Citations
```
In differential calculus, function is given and the differential is obtained.
(`
  Name = Alfred Hitchcock
  Title = Strangers on a Train
  Date = 2020 Feb 07
)

If you see a turtle on a fence post, you know it didn't get there by itself.
(`
  Title = Mathematical Challenges to Darwin's Theory of Evolution
  Available = https://www.youtube.com/watch?v=noj4phMT9OE
  Date = 2020 Feb 20
)

```
### Log Events
```
(`
  event-type = update-user-email
  timestamp = 2020-02-22T00:00:00.000Z
  user-id = user123

  [payload]
  email = xxx@xxx.com
)
```
### Track Spending
```
@buy laptop
(`
  price = 1500
  currency = USD
  date = 2020 Feb 23
)

```
### Track Ideas
```
@learn Hedonic Quality Adjustments
(`
  name = Wolfe Richter
  title = What Worries me About "Hedonic Quality Adjustments"
  available = https://www.youtube.com/watch?v=YHMhex4o0n4
  date = 2020 Feb 22
)
@update more thoughts (`date = 2020 Feb 23)

```
### Record Diary
```
move interstate (`date = 2020-02-22)
@update my new address is 123 New City Street, New City, 1234 (`date = 2020-02-23)

spend time with family (`date = 2020-02-23)
```
