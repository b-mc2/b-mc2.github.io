# ETL (Load)



---
## Flat Files
### Python Print Statement 
[Documentation](https://stackoverflow.com/a/36571602)

Give print a file keyword argument, where the value of the argument is a file stream. We can create a file stream using the open function
```python
# use file=open() to direct the output to the file location
# using "a" appends or creates the file

print("Hello World!", file=open("output.txt", "a"))

some_text = "This is a string of something I want to save"

print(some_text, file=open("output.txt", "a"))
```
Using `a` with python's `open()` command will append the data to a new line separated by a line break
this actually is exactly how `JSONL` documents are formated, where each new line is valid JSON. So running the 
following will build valid `JSONL` documents.

```python
some_dict = {'Name':"Alice"}

print(json.dumps(some_dict), file=open("logs.jsonl", "a"))
```