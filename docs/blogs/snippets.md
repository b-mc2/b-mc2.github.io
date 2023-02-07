# Cool Snippets of Python Code

Here's some snippets of Code that are cool to reuse.

## Python
I'm convinced that `map`, `filter`, `lambda`, `zip`, `functools` and `itertools` are some of the most 
powerful functions/tools within Python and I'd like to expand my capabilities with them and have been trying to solve coding 
challenges using them exclusively. Here's some scripts using these, while they aren't all super readable in one-line 
form, they are fast and easy to make into lambda functions.

---

#### Create a list of combined strings from multiple iterables
```python
list(map(lambda a,b: f"{a}-{b}", ('apple', 'banana', 'cherry'), ('orange', 'lemon', 'pineapple')))
# ['apple-orange', 'banana-lemon', 'cherry-pineapple']
```

#### Filter a list by min value
```python
ages = [5, 12, 17, 18, 24, 32]

[age for age in ages if age >= 18]
# or
list(filter(lambda age: age >= 18, ages))
# [18, 24, 32]
```

#### Create a list of sums of two lists by iterables 
```python
list(map(sum, zip([1,2,3], [4,5,6])))
# [5, 7, 9]
```

#### Join two lists together into list of tuples
```python
zipped_list = list(zip([1,2,3], [4,5,6]))
# [(1, 4), (2, 5), (3, 6)]
```

#### Unzip a list of tuples
```python
list(zip(*zipped_list))
# [(1, 2, 3), (4, 5, 6)]
```

#### Unzip list of lists
```python
list(map(list, zip(*zipped_list)))
# [[1, 2, 3], [4, 5, 6]]
```

#### Create dictionary with key,values from two lists
```python
dict(zip(['a','b','c'], [1,2,3]))
# {'a': 1, 'b': 2, 'c': 3}
```

#### Create dictionary with Index number keys
```python
some_list = ['Alice', 'Liz', 'Bob']
# One-Line Statement Creating a Dict:
dict(enumerate(some_list))
# {0: 'Alice', 1: 'Liz', 2: 'Bob'}

# or as a function
indexed_dict = lambda some_list: dict(enumerate(some_list))
indexed_dict(some_list)
# {0: 'Alice', 1: 'Liz', 2: 'Bob'}
```

#### Create frequency map of character counts in a string
```python
freq_map = lambda some_str: dict(zip(some_str, map(lambda x: some_str.count(x), some_str)))
freq_map('Hello World')
# {'H': 1, 'e': 1, 'l': 3, 'o': 2, ' ': 1, 'W': 1, 'r': 1, 'd': 1}
```

#### Get max value for each index number across multiple lists
```python
list(map(max, zip([10,12,32], [14,5,16])))
# [14, 12, 32]
```

#### Sort list of dictionaries by specific value
```python
cars = [{'car': 'Ford', 'year': 2005}, {'car': 'Mitsubishi', 'year': 2000}, {'car': 'BMW', 'year': 2019}]

cars.sort(key = lambda x: x['year'])
# [{'car': 'Mitsubishi', 'year': 2000}, {'car': 'Ford', 'year': 2005}, {'car': 'VW', 'year': 2011}]

# pretty print sorted list of dictionaries
# Change order by adding `reverse=True` to `sorted()`
print(json.dumps(sorted(cars, key=lambda x: x['year']), indent=4))
# [
#     {
#         "car": "Mitsubishi",
#         "year": 2000
#     },
#     {
#         "car": "Ford",
#         "year": 2005
#     },
#     {
#         "car": "BMW",
#         "year": 2019
#     }
# ]

```

#### One liner to output a pretty JSON file
```python
print(json.dumps(data, indent=4), file=open("path/to/data.json", 'w'))
```

#### One liner to output a JSONL file, run multiple times to keep adding
```python
print(json.dumps(data), file=open("path/to/data.jsonl", 'a'))
```

#### Calculate Dot product without dependencies
```python
sum(map(lambda x: x[0] * x[1], zip([1,2,3], [4,5,6])))
# 32
# Or save as a function
dot = lambda a, b: sum(map(lambda x: x[0] * x[1], zip(a, b)))
dot(a, b)
# 32
```

#### Strip non alphabet characters from strings
```python
remove_chars = lambda some_str: "".join([x for x in some_str if x.isalpha() or x == " "])
# or
remove_chars = lambda some_str: "".join((filter(lambda x: x.isalpha() or x == " ", some_str)))
remove_chars(")#$Hello3 World@$)")
# Hello World
```

#### check if N is divisible by X AND Y
```python
n_divisible = lambda n, x, y: all([divmod(n, x)[1] == 0, divmod(n, y)[1] == 0])
n_divisible(12, 2, 6)
# True
```

#### Get number of odd numbers below N
```python
# Faster
odd_below = lambda n: len([x for x in range(n) if x % 2 != 0])
# or a bit slower
odd_below = lambda n: len(list(filter(lambda x: x % 2 != 0, range(n))))
odd_below(15)
# 7
```

#### Get pair with smallest/largest difference between two iterables
[g4g](https://www.geeksforgeeks.org/python-record-point-with-minimum-difference/)
```python
# smallest
smallest_pair = min(some_list, key = lambda sub: abs(sub[1] - sub[0]))
# largest
largest_pair = max(some_list, key = lambda sub: abs(sub[1] + sub[0]))
```

#### Factoral (Recursive)
[blog.finxter](https://blog.finxter.com/python-one-liners/)
```python
factoral = lambda n: 1 if n <= 1 else n * factoral(n - 1)
print(factoral(10))
# 3628800
```

#### Prime Number Check (Naive)
```python
prime = lambda n: any([i for i in range(2, n) if n % i == 0]) != True

prime(27)
# False
primt(5)
# True
```

#### Prime Number Check (Optimized)
Optimization: checking if ending digit divisible by 2, reduced search space by half
```python
prime = lambda n: False if int(str(n)[-1]) % 2 == 0 else any([i for i in range(2, int(n/2)+1) if n % i == 0]) != True

prime(27)
# False
prime(4_589_407)
# True
```


#### Mersenne Prime Number Finder (Danger!)
[Info on 2,147,483,647](https://en.wikipedia.org/wiki/2,147,483,647)
```python
prime = lambda n: False if int(str(n)[-1]) % 2 == 0 else any([i for i in range(2, int(n/2)+1) if n % i == 0]) != True
mersenne = lambda n: list(filter(lambda i: i is not None, list(map(lambda x: 2**x -1 if prime(2**x -1) else None, range(1, n+1)))))

mersenne(31)
# [1, 3, 7, 31, 127, 8191, 131071, 524287, 2147483647]
# This took ~1.5 minutes to run
```


#### Quick Ratio (rough) without dependencies
```python
quick = lambda a,b: len(list(filter(lambda x: x[0] == x[1], zip(a,b)))) / (len(a+b)/2)

quick('dog', 'dog')
# 1.0
quick('dog', 'fog')
# 0.6666666666666666
quick('dog', 'cat')
# 0.0
quick('dog', 'dogmatic')
# 0.5454545454545454
```

#### ROT13 Cipher in a one line lambda
```python
rot13 = lambda word: "".join(list(map(lambda l: chr(((ord(l)-84) % 26) + 97), word)))
rot13('weattackatdawn')
# jrnggnpxngqnja
rot13('jrnggnpxngqnja')
# weattackatdawn
```

#### One-Time Pad in one line lambda (why?)
```python
otp = lambda message: list(zip(*list(map(lambda l: (chr(((ord(l[0])+l[1]))), l[1]), list(map(lambda x: (x, random.randint(1,10)),message))))))
# returns a list of two tuples, first is characters, second is keys
response = otp('Hello World!')
key = response[1]
# key = (3, 5, 1, 10, 10, 7, 2, 1, 10, 10, 5, 2)
cipher = "".join(response[0])
# cipher = Kjmvy'Yp|vi#

# decrypt
"".join(chr(ord(letter) - key) for letter, key in zip(cipher, key))
# 'Hello World!'
```


#### Input multiple lines (often used in competitions)
```python
# Takes in space separated integers as strings, maps to ints
K, M = map(int, input().split())
# 5 100
```

#### Regex for Zipcodes
[Stackoverflow](https://stackoverflow.com/questions/2577236/regex-for-zip-code)
```python
re.search(r"^\d{5}(?:[-\s]\d{4})?$", some_str)
# ^ = Start of the string.
# \d{5} = Match 5 digits (for condition 1, 2, 3)
# (?:…) = Grouping
# [-\s] = Match a space (for condition 3) or a hyphen (for condition 2)
# \d{4} = Match 4 digits (for condition 2, 3)
# …? = The pattern before it is optional (for condition 1)
# $ = End of the string.
```

#### Regex for Phone Numbers
[Stackoverflow](https://stackoverflow.com/questions/37393480/python-regex-to-extract-phone-numbers-from-string)
```python
re.findall(r'[\+\(]?[1-9][0-9 .\-\(\)]{8,}[0-9]', some_str)
```
