# Phone numbers

## Source
The rules these regex'es abide by come from two wikipedia articles about danish telephone numbers:

[National conventions for writing telephone numbers](https://en.wikipedia.org/wiki/National_conventions_for_writing_telephone_numbers#Denmark)

[Telephone numbers in Denmark](https://en.wikipedia.org/wiki/Telephone_numbers_in_Denmark)

## Rules

### End-user format - National version

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

### End-user format - International version

Rules used are the same as the National version
but always having a +45 prefix followed by a space.
```regex
(?:\+?45 ?)(?:(?:\d{8})|(?:\d{2} \d{2} \d{2} \d{2})|(?:\d{4} \d{4})|(?:\d{2} \d{3} \d{3}))
```

Matches

+4512345678 | +45 1234 5678 | +45 12 34 56 78 | +45 12 345 678

Non-Matches

12345678 | 1234 5678 | 12 34 56 78 | 12 345 678

123 456 78 | 12 3456 78 | 1 23 45 67 8 | 55 ab 66 xy

+45 123 456 78 | +45 12 3456 78 | +45 1 23 45 67 8 | +45 55 aa 66 bb

### End-user format - Mixed version (National + International)

This version matches both national and international phone number patterns.

```regex
(?:\+?45 ?|)(?<!\+)(?:(?:\d{8})|(?:\d{2} \d{2} \d{2} \d{2})|(?:\d{4} \d{4})|((?!\+)(?:\d{2} \d{3} \d{3}))|((?:\d{8})|(?:\d{2} \d{2} \d{2} \d{2})|(?:\d{4} \d{4})|(?:\d{2} \d{3} \d{3})))
```

Matches

12345678 | 1234 5678 | 12 34 56 78 | 12 345 678

+4512345678 | +45 1234 5678 | +45 12 34 56 78 | +45 12 345 678

Non-Matches

123 456 78 | 12 3456 78 | 1 23 45 67 8 | 55 ab 66 xy

+45 123 456 78 | +45 12 3456 78 | +45 1 23 45 67 8 | +45 55 aa 66 bb

### Stored format - National

For stored raw phone numbers only comprising of 8 digits.

```regex
\d{8}
```

Matches

12345678

Non-Matches

+45 1234 5678 | +45 12 34 56 78 | +45 12 345 678

1234 5678 | 12 34 56 78 | 12 345 678

123 456 78 | 12 3456 78 | 1 23 45 67 8 | 55 ab 66 xy

+45 123 456 78 | +45 12 3456 78 | +45 1 23 45 67 8 | +45 55 aa 66 bb

### Stored format - International

For stored phone numbers starting with the 45 country code.

```regex
(?:45)(?:\d{8})
```

Matches

+4512345678 4512345678

Non-Matches

+45 1234 5678 | +45 12 34 56 78 | +45 12 345 678

12345678 | 1234 5678 | 12 34 56 78 | 12 345 678

123 456 78 | 12 3456 78 | 1 23 45 67 8 | 55 ab 66 xy

+45 123 456 78 | +45 12 3456 78 | +45 1 23 45 67 8 | +45 55 aa 66 bb

### Stored format - Mixed (National and International)

Note: It is strongly advised not to use a mixed format for storing data.

This is kept here to help matching existent data that for one reason or the other has both formats.

```regex
((?:45\d{8})|(?:\d{8}))
```

Matches

4512345678 12345678

Non-Matches

+45 1234 5678 | +45 12 34 56 78 | +45 12 345 678

1234 5678 | 12 34 56 78 | 12 345 678

123 456 78 | 12 3456 78 | 1 23 45 67 8 | 55 ab 66 xy

+45 123 456 78 | +45 12 3456 78 | +45 1 23 45 67 8 | +45 55 aa 66 bb
