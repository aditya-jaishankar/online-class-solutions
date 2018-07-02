# Introduction to Computer Science and Programming Using Python (MIT's 6.00.1x)

## Course Syllabus

### Exercise Problems

#### 1) Longest substring

Given a string, find the longest substring whose characters are in alphabetical order

##### Solution

```python
def longest_alphabetical_substring(s):
    main_counter, main_longest = 0, ''
    while main_counter < len(s)-1:
        sub_counter, sub_longest = main_counter, s[main_counter]
        while sub_counter < len(s)-1:
            if s[sub_counter] > s[sub_counter+1]:
                main_counter = sub_counter
                break
            else:
                sub_longest += s[sub_counter+1]
                sub_counter += 1
        if len(sub_longest) > len(main_longest):
            main_longest = sub_longest
        main_counter += 1
    print('Longest substring in aplhabetical order is: ', main_longest)
    return
```
