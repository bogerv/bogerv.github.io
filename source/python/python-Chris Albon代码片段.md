---
title: python-Chris Albon代码片段
date: 2020-12-05 11:01:50
categories: [python]
---

## 格式化字符串

```python
import sys

'This is %d %s bird!' % (1, 'dead')

'%(number)d more %(food)s' % {'number' : 1, 'food' : 'burger'}

'My {1[kind]} runs {0.platform}'.format(sys, {'kind': 'laptop'})
```

字符串格式化代码

- %s string
- %r repr string
- %c character (integer or string)
- %d decimal
- %i integer
- %x hex integer
- %X same as X but with uppercase
- %e floating point lowercase
- %E floating point uppercase
- %f floating point decimal lowercase
- %F floating point decimal uppercase
- %g floating point e or f
- %G floating point E or F
- %% literal %

## 字符串索引

```python
string = 'Strings are defined as ordered collections of characters.'
string[:] # 'Strings are defined as ordered collections of characters.'
string[0:3] # 'Str'
# Print the first to the tenth character, skipping every other character
string[0:10:2] # 'Srnsa'
# 反转字符串
string[::-1] # '.sretcarahc fo snoitcelloc deredro sa denifed era sgnirtS'
```

## 处理 Long Lines 代码

```python
member_years_by_age = [first_list_element * second_list_element for first_list_element, second_list_element in zip(ages_of_community_members, number_of_ages)]
```

虽然可以使用 `\` 来分割代码, 更易读的方式是在 [] {}中使用注释, 因为在 [], {}中换行符会被忽略.

```python
# Create a variable with the count of members per age
member_years_by_age = [  # multiple the first list's element by the second list's element
    first_list_element * second_list_element
    # for the first list's elements and second list's element
    for first_list_element, second_list_element
    # for each element in a zip between the age of community members
    in zip(ages_of_community_members,
            # and the number of members by age
            number_of_ages)
]
```

## 使用默认字典

```python
import collections

# Create a defaultdict with the default value of 0 (int's default value is 0)
arrests = collections.defaultdict(int)
# Add an entry of a person with 10 arrests
arrests['Sarah Miller'] = 10
# Add an entry of a person with no value for arrests,
# thus the default value is used
arrests['Bill James']
# View dictionary
arrests

defaultdict(int, {'Bill James': 0, 'Sarah Miller': 10})
```
