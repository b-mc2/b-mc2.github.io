# File Formats

Data has to be stored somehow, here are some useful resources for popular data formats for the web as well as storage.

---
### CSV
[Documentation](https://docs.fileformat.com/spreadsheet/csv/)

Files with .csv (Comma Separated Values) extension represent plain text files that contain records of data with comma separated values. Each line in a CSV file is a new record from the set of records contained in the file.

Pretty straightforward and very common. CSV doesn't retain data types so storage space isn't optimal. 

Each record is located on a separate line, delimited by a line break (CRLF).  For example:
```
aaa,bbb,ccc CRLF
zzz,yyy,xxx CRLF
```
---
### LOG
[Documentation](https://docs.fileformat.com/database/log/)

A file with `.log` extension contains the list of plain text with timestamp. Usually, certain activity detail is logged by the softwares or operating systems to help the developers or users to track what was happening at a certain time period. Users can edit these files very easily by using any text editors. Usually the error reports or login activities are logged by the operating systems, but other softwares or web servers also generate log files to track visitors and to monitor bandwidth usage.

---
### SQLite
[Documentation](https://docs.fileformat.com/database/sqlite/)

A file with `.sqlite` extension is a lightweight SQL database file created with the SQLite software. It is a database in a file itself and implements a self-contained, full-featured, highly-reliable SQL database engine. SQLite database files can be used to share rich contents between systems by simple exchanging these files over the network. Almost all mobiles and computers use SQLite for storing and sharing of data, and is the choice of file format for cross-platform applications. Due to its compact use and easy usability, it comes bundled inside other applications. SQLite bindings exist for programming languages such as C, C#, C++, Java, PHP, and many others.

SQLite in reality is a C-Language library that implements the SQLite RDBMS using the SQLite file format. With the evolution of new devices every single day, its file format has been kept backwards compatible to accommodate older devices. SQLite file format is seen as long-term archival format for the data.

---
### SQL
[Documentation](https://docs.fileformat.com/spreadsheet/csv/)

A file with `.sql` extension is a Structured Query Language (SQL) file that contains code to work with relational databases. It is used to write SQL statements for CRUD (Create, Read, Update, and Delete) operations on databases. SQL files are common while working with desktop as well as web-based databases. There are several alternatives to SQL such as Java Persistence Query Language (JPQL), LINQ, HTSQL, 4D QL, and several others. SQL files can be opened by query editors of Microsoft SQL Server, MySQL and other plain text editors such as Notepad on Windows OS.

SQL files are in plain text format and can comprise of several language elements. Multiple statements can be added to a single SQL file if their execution is possible without depending on each other. These SQL commands can be executed by query editors for carrying out CRUD operations.

---
### Feather
[Documentation](https://arrow.apache.org/docs/python/feather.html)

Feather is a portable file format for storing Arrow tables or data frames (from languages like Python or R) that utilizes the Arrow IPC format internally. Feather was created early in the Arrow project as a proof of concept for fast, language-agnostic data frame storage for Python (pandas) and R.


[Additional Info](https://towardsdatascience.com/stop-using-csvs-for-storage-this-file-format-is-150-times-faster-158bd322074e)

---
### Parquet
[Documentation](https://parquet.incubator.apache.org/documentation/latest/)

Apache Parquet has the following characteristics:

- Self-describing
- Columnar format
- Language-independent 

Self-describing data embeds the schema or structure with the data itself. Hadoop use cases drive the growth of self-describing data formats, such as Parquet and JSON, and of NoSQL databases, such as HBase. These formats and databases are well suited for the agile and iterative development cycle of Hadoop applications and BI/analytics. 

Optimized for working with large files, Parquet arranges data in columns, putting related values in close proximity to each other to optimize query performance, minimize I/O, and facilitate compression. Parquet detects and encodes the same or similar data using a technique that conserves resources.

[Additional Info](https://towardsdatascience.com/csv-files-for-storage-no-thanks-theres-a-better-option-72c78a414d1d)

---
### Avro
[Documentation](https://www.tutorialspoint.com/avro/avro_overview.htm)

Apache Avro is a language-neutral data serialization system. Avro has a schema-based system. A language-independent schema is associated with its read and write operations. Avro serializes the data which has a built-in schema. Avro serializes the data into a compact binary format, which can be deserialized by any application.

Avro uses JSON format to declare the data structures. Presently, it supports languages such as Java, C, C++, C#, Python, and Ruby.


### JSONL
[Documentation](https://jsonlines.org/)

JSON Lines is a convenient format for storing structured data that may be processed one record at a time. It works well with unix-style text processing tools and shell pipelines. It's a great format for log files. It's also a flexible format for passing messages between cooperating processes.

The JSON Lines format has three requirements:
- UTF-8 Encoding
  - JSON allows encoding Unicode strings with only ASCII escape sequences, however those escapes will be hard to read when viewed in a text editor.
- Each Line is a Valid JSON Value
  - The most common values will be objects or arrays, but any JSON value is permitted.
- Line Separator is `\n`
  - This means '\r\n' is also supported because surrounding white space is implicitly ignored when parsing JSON values. The last character in the file may be a line separator, and it will be treated the same as if there was no line separator present.

Data as JSONL:
```json
{"name": "Gilbert", "wins": [["straight", "7♣"], ["one pair", "10♥"]]}
{"name": "Alexa", "wins": [["two pair", "4♠"], ["two pair", "9♠"]]}
{"name": "May", "wins": []}
{"name": "Deloise", "wins": [["three of a kind", "5♣"]]} 
```