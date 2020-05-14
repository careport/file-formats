# Patient Roster Specification Version 2 (errata 3)

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
not count as delimiters; they are part of the field value. If you need
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

A roster file's name has six parts:
- the roster name
- the reporting period duration
- the reporting period start date
- a timestamp indicating when the file was generated
- the patient roster specification version
- the file extension, which should always be `csv`

The various parts of the file name are separated by underscores (`_`),
except for the file extension, which, as usual, is separated from the
rest of the file name by a period (`.`).

More on each:

- The roster name is just a short, descriptive string that identifies the roster, e.g., "ACO" or "northeast." It should contain only letters and numbers.
- The reporting period duration is either `day`, `month`, `quarter`, or `year`.
- The reporting period start date is the first date of the reporting
  period in ISO 8601 basic date format. For example, `20200101` is the
  first of January, 2020.
- The timestamp is a UTC date and time in ISO 8601 basic date/time format. For example, `20200114T190400Z` is January 14, 2020 at 19:04 (i.e., 7:04PM) in UTC, or the same date at 3:04PM in US Eastern time.
- The patient roster specification version is the version number of this document, preceded by `v`. For example, `v2` indicates that the file conforms to version 2 of the patient roster specification.

Putting those together, the file name would be:

 `ACO_month_20200101_20200114T190400Z_v2.csv`

Together, the reporting period duration and start date indicate that the patients listed in the file are members of the roster for that period. In our example above, each patient in the file would be considered a member of the `ACO` roster for the entire month of January 2020.


## Headers

A patient roster does not require a header line, but you can provide one if you wish.


## Fields

Each record in a patient roster file contains the following fields, in the following order:

1. \* [Type of unique patient identifier](#unique-id-type)
2. \* Unique patient identifier
3. \* Last name
4. \* First name
5. Middle name
6. Name suffix (e.g., `Jr.`, `III`, etc.)
7. \* [Sex](#sex-format)
8. \* [Date of birth](#date-format)
9. +[Social security number (SSN)](#ssn-format)
10. +[Home phone number](#phone-format)
11. +Address line 1
12. Address line 2
13. \*City
14. \*State
15. \*[ZIP code](#zip-format)
16. Care coordinator name
17. Care coordinator [phone number](#phone-format)
18. Care coordinator email
19. Primary care physician name
20. Primary care physician NPI
21. Primary care physician [phone number](#phone-format)
22. Primary care physician email
23. Primary payer
24. Primary payer patient identification number
25. [Tag 1](#tag-details)
26. Tag 2
27. Tag 3
28. Tag 4
29. Tag 5
30. Tag 6
31. Tag 7
32. Tag 8
33. Tag 9
34. Tag 10
35. +Patient email
36. +[Cell phone number](#phone-format)
37. +[Work phone number](#phone-format)
38. +[Medicare Beneficiary Identifier (MBI)](#mbi-format)

Fields marked with (\*) are required to have non-empty values. If you
have the data marked with (+), we strongly encourage you to send it,
since it will greatly enhance our ability to provide you with more
accurate information about your patients.


### Field formats
#### <a name="unique-id-type"></a>Type of unique patient identifier

This is a short code (usually 2-4 characters in length), in all
capital letters, that indicates what kind of unique id you are
providing for each patient in your roster. You can find a list of such
codes [here](https://hl7-definition.caristix.com/v2/HL7v2.5.1/Tables/0203).

You are most likely to use one of the following:

- `MR`: medical record number
- `PI`: patient internal identifier
- `PT`: patient external identifier
- `MB`: member number, indicating an insured member of an insurance policy
- `SN`: subscriber number, indicating the subscriber insurance
policy<sup>†</sup>
- `SS`: Social Security number
- `MA`: Medicaid number
- `MC`: Medicare number

If none of these fit, and you are unsure of what to use, please ask
us.

<sup>†</sup> If your roster contains multiple individuals who are all
covered by the same subscriber's policy, then you'll probably want to
use `MB` instead of `SN`, because a given patient identifier must
appear only once in the entire file.

#### <a name="date-format"></a>Date format

The date of birth should be in the ISO 8601 _extended_ date format, `yyyy-mm-dd`, e.g., `2018-03-14`.


#### <a name="sex-format"></a>Sex format

Five different values are allowed for `sex`:
- `female`
- `male`
- `other`
- `unknown`
- `ambiguous`

These values are not case-sensitive. That is, `female`, `FEMALE`, and
`Female` all mean the same thing. Additionally, each option may be
abbreviated by its first letter. So, the all of the values in each row
of the following table have the same meaning:

|           |           |           |   |   |
|-----------|-----------|-----------|---|---|
| FEMALE    | female    | Female    | f | F |
| MALE      | male      | Male      | m | M |
| OTHER     | other     | Other     | o | O |
| UNKNOWN   | unknown   | Unknown   | u | U |
| AMBIGUOUS | ambiguous | Ambiguous | a | A |


#### <a name="ssn-format"></a>SSN format

An SSN may be written with the traditional hyphenation or without.
So, `123-45-6789` and `123456789` are both allowed.  Unknown or not applicable SSN should be omitted.


#### <a name="phone-format"></a>Phone format

Phone numbers are expected to be 10-digit US phone numbers, with
optional parentheses, dashes, and spaces as delimiters. The following
are all valid formats for the same phone number:
- `(123) 456-7890`
- `123-456-7890`
- `1234567890`

This format does not allow extensions or non-US phone
numbers. Extensions should be removed, and non-US phone numbers should
be omitted altogether.

The home phone number should be submitted if cell phone number is
submitted, even if they are the same number — i.e. submit the same
number in both.


#### <a name="zip-format"></a>ZIP code format

A ZIP code may be formatted either as a 5-digit ZIP or a hyphenated ZIP+4.
So, `12201` and `12201-7050` are both valid.


#### <a name="tag-details"></a>Tags

You may provide up to 10 tags for each row in the CSV. CarePort will
assign these tags as attributions for the patient identified in the row.
Attributions enable end users to segment patient populations in searches,
reports, and alerts.


#### <a name="mbi-format"></a>MBI format

The syntax of a Medicare Beneficiary Id is specified by
CMS in
[this document](https://www.cms.gov/Medicare/New-Medicare-Card/Understanding-the-MBI-with-Format.pdf).

We will accept MBIs that use upper or lower-case letters, with or
without dashes, but internally we will normalize them to all-caps
without dashes.

Note that a Medicate Beneficiary Id (MBI) is _not_ the same thing as an old-
style Health Insurance Claim Number (HICN). HICNs are based on Social
Security Numbers, but MBIs are not.
