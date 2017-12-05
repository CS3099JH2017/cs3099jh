## Formal CSV Specification

ML should only expect to handle data in the format specified below, and BE
should only ever pass ML data in this format:

Taken from CSV specification proposed by RFC4180 with a few amendments:

A CSV is plain text file using the character set UTF-8 that:
- consists of one record per line.
- with the records divided into fields separated by a comma.
- where every record has the same number and sequence of fields.

Additionally, the first row of the CSV must give the names/labels of each field. 
If a comma is part of a field, then the field must be surrounding by double quotes (").
If a double quote is part of a field, then it must be escaped with a back slash (\\), 
regardless of whether or not the field is quoted.

ML should expect to deal with CSVs of this format, and BE should only pass CSVs of this 
format to ML. If any other format is passed to ML, it is valid behaviour to throw an error.

Example of a valid record:
```testing, "1, \"quoted\"", help \"me\"```
