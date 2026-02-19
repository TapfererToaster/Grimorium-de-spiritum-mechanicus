>[!note]
>The website that hosts the YAML specification (https://www.yaml.org) explicitly states that YAML stands for YAML Ain’t Markup Language and that “YAML is a human-friendly data serialization language for all programming languages.” So, YAML is primarily intended as a data serialization language, with the added goal of being as human-friendly as possible.

# YAML 

## Start and End of a Document
The beginning of a document is marked by three dashes `---`, but they are not strictly required.
The end of an YAML file is marked by three dots `...`, but no error is thrown if they are not written.
## Comments
Comments are start with a hashmark `#` like in Python or Ruby.
## Indentation and Whitespace
Like Python, YAML uses space indentation.
## Strings
Strings do not need to be quoted in YAML. A good practice nonetheless is to double-quote variables and other expressions; and single-qoute values that should not be evaluated, like version numbers.
## Booleans 
YAML provides a variety of boolean values, but you can just use `true` and `false`.
## Lists
YAML calls lists *sequences*, but the offical Ansible documentation still refers to lists.
Lists have a name followed by a colon, you then indent list items and delimit them with hyphens:
```
shows:
	- My fair Lady
	- Oklajoma
	- The Pirates of Penzance
```

You can also use the inline format:
```
shows: [My fair Lady, Oklahoma, The Pirates of Penzance]
```

## Dictionaries
YAML call dictionaries *mappings*, but the offical Ansible documentation still refers to them as dictonaries.
The syntax is:
```
address:
	street: Main Street
	appt: 742
	city: Logan
	state: Ohio
```

You can also use the inline format:
```
address: {street: Main Street, appt: '742', city: Logan, state: Ohio}
```
## Multiline Strings
You can format multiline strings with YAML by combining a block style indicator (`|` or `>`), a block chomping indicator (`+` or `-`) and even an indentation indicator (`1` to `9`).
For example, when we need a a preformatted block, we use the pipe character with a plus sign `|+`, telling the YAML parser to keep all line breaks as entered:
```
---
visiting_address: |+
	Department of Computer Science
	
	A.V. Williams Building
	University of Maryland
city: College Park
state: Maryland
```