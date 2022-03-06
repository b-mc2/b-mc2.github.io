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