# Phone numbers

## Source
The rules these regex'es abide by come from two wikipedia articles about danish telephone numbers:

[National conventions for writing telephone numbers](https://en.wikipedia.org/wiki/National_conventions_for_writing_telephone_numbers#Denmark)

[Telephone numbers in Denmark](https://en.wikipedia.org/wiki/Telephone_numbers_in_Denmark)

## Rules

End-user format - National version:

A phone number can be written as:
* A sequence of 8 digits, like 12345678
* Two groups of 4 digits delimited by a space, like 1234 5678
* Four groups of 2 digits delimited by a space, like 12 34 56 78
* Three groups, one of 2 digits and the next of 3 digits, like 12 345 678
```regex
(?:\d{8})|(?:\d{2} \d{2} \d{2} \d{2})|(?:\d{4} \d{4})|(?:\d{2} \d{3} \d{3})
```
Matches 	

12345678 | 1234 5678 | 12 34 56 78 | 12 345 678

Non-Matches 	

123 456 78 | 12 3456 78 | 1 23 45 67 8 | 55 ab 66 xy
