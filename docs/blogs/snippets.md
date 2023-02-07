# Cool Snippets of Python Code

Here's some snippets of Code that are cool to reuse.

## Python
I'm more and more convinced that `map`, `filter`, `lambda` and `zip` are some of the most powerful functions within Python
and I'd like to expand my capabilities with them and have been trying to solve coding challenges using them exclusively.
Here's some scripts using these, while they aren't all super readable in one-line form, they are fast and easy to make
into lambda functions.

---

#### Create a list of combined strings from multiple iterables
```python
list(map(lambda a,b: f"{a}-{b}", ('apple', 'banana', 'cherry'), ('orange', 'lemon', 'pineapple')))
# ['apple-orange', 'banana-lemon', 'cherry-pineapple']
```

#### Filter a list by min value
```python
ages = [5, 12, 17, 18, 24, 32]
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
# called like dot(a,b) returns 32
```

#### Strip non alphabet characters from strings
```python
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

odd_below = lambda n: len(list(filter(lambda x: x % 2 != 0, range(n))))
odd_below(15)
# 7
```

#### Rough Quick Ratio without dependencies
```python
quick = lambda a,b: len(list(filter(lambda x: x[0] == x[1], zip(a,b)))) / (len(a+b)/2)
```

#### ROT13 Cipher in a one line lambda
```python
rot13 = lambda word: "".join(list(map(lambda l: chr(((ord(l)-84) % 26) + 97), word)))
rot13('weattackatdawn')
# jrnggnpxngqnja
rot13('jrnggnpxngqnja')
# weattackatdawn
```