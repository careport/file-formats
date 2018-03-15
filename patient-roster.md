# Patient Roster

## CSV

The overall format of a patient roster is CSV (comma-separated values), as defined by [IETF RFC 4180](https://tools.ietf.org/html/rfc4180).
Please refer to the RFC for a complete description. An informal description follows:

A CSV file is a sequence of records, one per line. A record is a
sequence of fields, which are separated by commas. For example:
```csv
Doe,John,Indiana
```
is a record containing three fields. The values of those fields are `Doe`, `John`, and `Indiana`.

A CSV file can start with an optional header line. The header follows
the same format as a normal record, but its fields contain the names
of each field in the body of the file. We can add a header to our
example:
```csv
Last name,First name,Address
Doe,John,Indiana
```

Since newlines and commas are special characters -- they delimit
records and fields, respectively -- they need to be escaped if they
appear within a field as part of the data. To escape the content of a
field, enclose the field value in double-quote characters. For
example, if we wanted to put John Doe's full address, commas and all,
in our third field, we could do this:
```csv
Last name,First name,Address
Doe,John,"123 Fake Street, Gary, Indiana 46401"
```
and the value of the `Address` field would be `123 Fake Street, Gary,
Indiana 46401`. As you can see, commas within a double-quoted field do
not count as delimters; they are part of the field value. If you need
to put a double-quote character within a double-quoted field value,
use two double-quote characters in a row. For example, let's add a
field to John's record:
```csv
Last name,First name,Address,Quotation
Doe,John,"123 Fake Street, Gary, Indiana 46401","""Exactly so,"" said Alice."
```
The value of the `Quotation` field is `"Exactly so," said Alice.`


## Character encoding

UTF-8 is the preferred character encoding for a patient roster, but we will accept files with other encodings.

When we set up a new patient roster, please tell us:
- the character encoding of the file
- whether or not the file starts with a BOM (byte-order mark)


## File name and reporting period

A roster file's name has five parts:
- the roster name
- the reporting period duration
- the reporting period start date
- a timestamp indicating when the file was generated
- the file extension, which should always be `csv`

The various parts of the file name are separated by underscores (`_`),
except for the file extension, which, as usual, is separated from the
rest of the file name by a period (`.`).

More on each:

- The roster name is just a short, descriptive string that identifies the roster, e.g., "ACO" or "northeast." It should contain only letters and numbers.
- The reporting period duration is either `month` or `week`.
- The reporting period start date is the first date of the reporting period in ISO 8601 basic date format. For example, `20180301` is the first of March, 2018.
- The timestamp is a UTC date and time in ISO 8601 basic date/time format. For example, `20180314T190400Z` is March 14, 2018 at 19:04 (i.e., 7:04PM) in UTC, or the same date at 3:04PM in US Eastern time.

Putting those together, the file name would be: `ACO_month_20180301_20180314T190400Z.csv`.

Together, the reporting period duration and start date indicate that the patients listed in the file are members of the roster for that period. In our example above, each patient in the file would be considered a member of the `ACO` roster for the entire month of March 2018.


## Headers

A patient roster does not require a header line, but you can provide one if you wish.


## Fields

Each record in a patient roster file contains the following fields, in the following order:

1. \* Type of unique patient identitifer (e.g, MRN, Insurance member ID, etc.)
2. \* Unique patient identifier
3. \* Last name
4. \* First name
5. Middle name
6. Name suffix (e.g., "Jr.," "III", etc.)
7. \* [Sex](#sex-format)
8. \* [Date of birth](#date-format)
9. [SSN](#ssn-format)
10. [Phone number](#phone-format)
11. Address line 1
12. Address line 2
13. City
14. State
15. [ZIP code](#zip-format)

Fields marked with (\*) are required to have non-empty values. Other
fields are allowed to be empty, but non-empty values will be useful
for matching patients.


### Field formats
#### <a name="date-format"></a>Date format

The date of birth should be in the ISO 8601 _extended_ date format, `yyyy-mm-dd`, e.g., `2018-03-14`.


#### <a name="sex-format"></a>Sex format

Five different values are allowed for `sex`:
- `female`
- `male`
- `other`
- `unknown`
- `ambiguous`

These values are not case-sensitive. That is, `female`, `FEMALE`, and `Female` all mean the same thing. Additionally, `female` may be abbreviated as `f` or `F`, and, similarly, `male` may be abbreviated as `m` or `M`.


#### <a name="ssn-format"></a>SSN format

An SSN may be written with the traditional hyphenation or without.
So, `123-45-6789` and `123456789` are both allowed.


#### <a name="phone-format"></a>Phone format

The preferred (but not required) format for US phone numbers is
`(123) 456-7890`.


#### <a name="zip-format"></a>ZIP code format

A ZIP code may be formatted either as a 5-digit ZIP or a hypenated ZIP+4.
So, `12201` and `12201-7050` are both valid.
