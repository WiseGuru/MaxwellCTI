---
{"dg-publish":true,"permalink":"/tool-deep-dives/python/python-introduction/"}
---

I started this section by following this video to ["Learn Python in 1 Hour"](https://www.youtube.com/watch?v=kqtD5dpn9C8), but then immediately got distracted by Python's potential and went down a rabbit hole.^[Or hognose hole, if you will.]

### Key Features and Definitions in Python
1. Case-sensitive
	1. "True" represents the Boolean true value, and "true" is just a string
2. Object-oriented
	1. Bundles attributes and functions to individual units (objects)
	2. Classes identify which attributes are used to define a type of object
		1. e.g., `class Hognose:`
	3. Methods are used by Classes to define or represent the specific attributes
		1. The method `def __init__(self,...)` defines attributes for the class
			1. e.g., `def __init__(self, name, age, length, ...)`
		2. The method `def __repr__(self):...` provides an "official" string *representation* of the object
			1. e.g., `def __repr__(self):`
			   `return (f"Hognose(name='{self.name}', age={self.age},...)`
3. Primitive data types
	1. Strings
		1. Unformatted, considered text
			1. `"12"` is a string, and cannot be added to or subtracted from
	2. Integers
		1. Whole numbers that can be manipulated with equations
		2. `1+1` will equate to `2`
		3. Can also convert strings to integers with `int()` (see example below)
			1. `birth_year = input("Enter your birth year (YYYY): ")     # Accepts the birth year from a user as a string`
			2. `int(birth_year)     # Converts the "birth_year" variable from a string to an integer`
	3. Float
		1. "Floating point number", which are rational numbers that usually end in decimal figures
		2. e.g., `3.14` or `0.5`
	4. Boolean
		1. Simple `True` and `False` or 1 and 0
		2. Boolean values^[[Boolean data type - Wikipedia](https://en.wikipedia.org/wiki/Boolean_data_type)]
		3. Function (`bool()`)
			1. Evaluates any value as "True" or "False"
				1. True in this case effectively refer to any non-null^[Null being the number 0, boolean False, the string "None", or an empty value] value
				2. Even `-1` returns True
4. Non-primitive data types (non-comprehensive)
	1. Lists
		1. Holds a large number of items of any data type
		2. Highly flexible; items can be added or removed
	2. Arrays
		1. Not natively supported in Python
		2. Like lists, but more efficient when handling large data sets of the same type
	3. Tuples
		1. Not a sex thing^[Probably.], but is an ordered list of values.
		2. There can be duplicates, and they are not changeable once created
	4. Dictionary
		1. A list of key-value pairs.
			1. The keys must be unique, but the values do not
				1. e.g., `dictionary = {'a': 1, 'b': 2, 'c': 1}`
				2. a, b, and c are the unique keys, followed by their values
5. Style guide
	1. PEP (Python Enhancement Proposals)
		1. [PEP 8 – Style Guide for Python Code | peps.python.org](https://peps.python.org/pep-0008/)
		2. This is a style guide with a bunch of specific recommendations 
	2. CapWord Convention
		1. When stringing a bunch of words together, capitalize the first letter of each word
			1. e.g. BigHairySpider
		2. Typically reserved for class names
			1. **NOTE**: Sometimes IDEs like PyCharm are too smart for their own good, and will read words like "Hognose" as a violation of this convention (it expects "HogNose")
			2. In PyCharm, if you're working with a term frequently, this can be resolved by adding it to PyCharm's dictionary
				1. Right-click the alert, select "Show Quick-Fixes", and then "Save 'Hognose' to dictionary"
	3. snake_case convention
		1. String words together with underscores `_` instead of capital letters
			1. e.g., big_hairy_spider
		2. Typically used for functions, methods, and variables
	4. Commenting
		1. Comments should be written as a sentence, begin with a capital letter, end with a period, and are identified with a pound and single space (`# `) at the start
		2. Block comments begin at the start of a line
			1. e.g., `# This is a comment.`
		3. Inline comments should be used sparingly
			1. Don't state the obvious
			2. e.g., `x = y + 1     # Compensate for image border.`

### Basic/Imporant Commands/tools
1. `print()`
	1. Print the contents of the parenthesis to the console
2. `variable = value`
	1. Declare a variable
	2. Used in code without quotes
		3. For example, let's say `nice = 69`
		4. `print(nice)` would print the value of `nice`, 69
		5. `print("nice")` would print the string "nice" (but without the quotes)
3. `datetime`
	1. [datetime — Basic date and time types — Python 3.12.2 documentation](https://docs.python.org/3/library/datetime.html)
	2. `strptime()` converts a string into a *datetime* object, and `strftime()` converts a *datetime* object into a string

### Hognose Census Project

Let's say we have a CSV list of hognose snakes with a bunch of data. The CSV was created by a human for humans, and has pretty long headers to help human operators input correct information.

We want to ingest the data from the CSV file, converting strings into their most appropriate value type (Boolean, integer, datetime, etc.) for maximum utility.

> For this project, I'm going to over-comment to help the tl;drs out there.

#### hognose_snakes.csv
```CSV
Snake Name,How old are they,What kind of snout,Are they good,Are they a good eater,When were they last fed
Sneaky,3,Boopable,Yes,True,3/3/2024
Hissy,5,Pointy,No,False,2/18/2024
Slithery,2,Soft,Yes,False,2/19/2024
```

#### Python Script
```Python
# Import required modules.
import csv
from datetime import datetime

# Create the "Hognose" class.
class Hognose:
# Define the attributes that make up the class.
    def __init__(self, name, age, snoot, goodness, good_eater, last_fed):
        self.name = name
        self.age = float(age)  # Convert age to a float number (to account for ages between 0 and 1).
        self.snoot = snoot
        self.goodness = goodness == 'Yes'  # Convert goodness to a boolean.
        self.good_eater = good_eater == 'True'  # Convert good_eater to a boolean.
        self.last_fed = datetime.strptime(last_fed, '%m/%d/%Y')  # Convert last_fed to a datetime object.

# Define the "official" string that is the snake object.
    def __repr__(self):
        return (f"Hognose(name='{self.name}', age={self.age}, snoot='{self.snoot}', "
                f"goodness={self.goodness}, good_eater={self.good_eater}, "
                f"last_fed='{self.last_fed.strftime('%m/%d/%Y')}')")


# Mapping between CSV headers and class attribute names.
header_mapping = {
    'Snake Name': 'name',
    'How old are they': 'age',
    'What kind of snout': 'snoot',
    'Are they good': 'goodness',
    'Are they a good eater': 'good_eater',
    'When were they last fed': 'last_fed'
}

# Read the CSV file and create Hognose instances.
hognose_snakes = []
with open('hognose_snakes.csv', 'r') as file:  # The 'r' stands for "read".
    csv_reader = csv.DictReader(file)
    for row in csv_reader:
        # Rename the keys based on the mapping.
        renamed_row = {header_mapping[key]: value for key, value in row.items()}
        # Create the Hognose object with the renamed keys.
        snake = Hognose(**renamed_row)
        hognose_snakes.append(snake)

# Print the list of Hognose objects.
for snake in hognose_snakes:
    print(snake)

```
