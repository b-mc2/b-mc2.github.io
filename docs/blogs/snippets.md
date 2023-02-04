# Cool Snippets of Python Code

Here's some snippets of Code that are cool to reuse.

## Python
```python
# Create a list of combined strings from multiple iterables
list(map(lambda a,b: f"{a}-{b}", ('apple', 'banana', 'cherry'), ('orange', 'lemon', 'pineapple')))
# ['apple-orange', 'banana-lemon', 'cherry-pineapple']

# filter a list by min value
ages = [5, 12, 17, 18, 24, 32]
list(filter(lambda age: age >= 18, ages))
# [18, 24, 32]

# create a list of sums of two lists by iterables 
list(map(sum, zip([1,2,3], [4,5,6])))
# [5, 7, 9]

# join two lists together into list of tuples
zipped_list = list(zip([1,2,3], [4,5,6]))
# [(1, 4), (2, 5), (3, 6)]

# unzip a list of tuples
list(zip(*zipped_list))
# [(1, 2, 3), (4, 5, 6)]

# unzip list of lists
list(map(list, zip(*zipped_list)))
# [[1, 2, 3], [4, 5, 6]]

# Create dictionary with key,values from two lists
dict(zip(['a','b','c'], [1,2,3]))
# {'a': 1, 'b': 2, 'c': 3}

# Get max value for each index number across multiple lists
list(map(max, zip([10,12,32], [14,5,16])))
# [14, 12, 32]

# Sort list of dictionaries by specific value
cars = [{'car': 'Ford', 'year': 2005}, {'car': 'Mitsubishi', 'year': 2000}, {'car': 'BMW', 'year': 2019}]
cars.sort(key = lambda x: x['year'])
# [{'car': 'Mitsubishi', 'year': 2000}, {'car': 'Ford', 'year': 2005}, {'car': 'VW', 'year': 2011}]

# One liner to output a pretty JSON file
print(json.dumps(data, indent=4), file=open("path/to/data.json", 'w'))

# One liner to output a JSONL file, run multiple times to keep adding
print(json.dumps(data), file=open("path/to/data.jsonl", 'a'))
```