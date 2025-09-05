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

This is kept here to help match existent data that for one reason or the other has both formats.

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

## Validation format

These regexes should be used to check if a given number in 8-digit, 3-digit, 4-digit or 5-digit is within the valid range of mobile numbers allowed in Denmark.
Note that these regexes would not allow 8-digit numbers like 00000000 or 12000000. They would meet the requirements for storage format but since they are not within a number assignment range they are not in use, and should not be for the foreseeable future.

### Validation format - Group 1 - National or Standard European Prefix

Allowed range 01****** - 09******

```regex
0[1-9]\d{6}
```
Matches:
01123456 | 05123456 | 09123456

Does not match:
00123456 | 10123456

### Validation format - Group 2 - Carrier preselect
```regex
10\d{2}
```

Matches:
1000 | 1011 | 1099

Does not match:
1100 | 1199 | 0120

### Validation format - Group 3 - 3-digit emergency numbers
Allowed range: 11*, 12*

```regex
1[1-2]\d{1}
```

Matches:
112 | 119 | 110 | 129 | 121

Does not match:
131 | 911 | 999 | 139 | 199

### Validation format - Group 4 - 4-digit short numbers

Allowed range: 18**

```regex
18\d{2}
```

Matches:
1800 | 1899 | 1834 | 1850

Does not match:
1900 | 1700 | 1000 | 2000

### Validation format - Group 5 - 5-digit Network access codes

Allowed range: 16***

```regex
16\d{3}
```

Matches:

16000 | 16999 | 16123

Does not match:
19000 | 17000 | 15000 | 20000

### Validation format - Group 6 - Mobile phones
Allowed ranges:

20****** – 31******

40****** – 42******

49******

50****** – 55******

60****** – 61******

71******

81******

91****** – 93******

```regex
(:?[2][0-9]|3[0-1]|4[0-2]|5[0-5]|6[0-1]|71|81|9[1-3])\d{6}|4911\d{4}
```

Matches:

20123456 | 29123456 | 31999999 | 30123456

40123456 | 42123456 | 41999999 | 42999999

49110000 | 49119999 | 49114567 | 49111234

50000000 | 55000000 | 51234567 | 54999999

60000000 | 60912345 | 61000000 | 61900000

71000000 | 71999999 | 71123456 | 71654321

81123456 | 81000000 | 81999999 | 81654321

91123456 | 92123456 | 93123456 | 93999999

Does not match:

00123456 | 01123456 | 32000000 | 39123456

43123456 | 43999999 | 49999999 | 45999999

49999999 | 49129999 | 49100000 | 49199999

56123456 | 57123456 | 58123456 | 59123456

63000000 | 63999999 | 69000000 | 69999999

72000000 | 72999999 | 79000000 | 70000000

80123456 | 82123456 | 80999999 | 89999999

90123456 | 94123456 | 99999999 | 90000000

### Validation format - Group 7 - Landline numbers

### Validation format - Group 8 - M2M numbers

### Validation format - Group 9 - Spare numbers

### Validation format - Group 10 - Split charge numbers

### Validation format - Group 11 - Free phone

### Validation format - Group 12 - Premium rate