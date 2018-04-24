## Naming Conventions
The Oracle World is awash with naming conventions, so here we will try to help you to decide how to create your own. 

Lets start with the best news in years: Oracle has broken the 30 character barrier! From 12c R2 on, 128 Bits are available for identifiers. Now we just have to refrain from using all of them!
### Good Naming Conventions:
 - Ensure unique names across all applications in the same DB
 - Use prefixes or postfixes but never both
 - Use abbreviations only for those prefixes or postfixes
 - Never combine more than 2 abbreviations 
 - Every name, except the abbreviations, reflects the enduser point of view
 - Define an abreviation dictionary
 - Use underscores instead of Camelcaps to facilitate automatic formatting
 - Distinguish SQL and PL/SQL
 - Facilitate (sub)project distinction
 - Facilitate object distinction
 - Can be checked automatically
 - Support automatic checks of other modelling/programming conventions
### Purpose
First purpose of naming conventions is to help programmers create better code.
Second purpose ist to help programmers to quickly understand each others code.

## Abbreviation Example
One good convention is to let the length of the abbreviation be decided by some overarching categorization: 
 1. Database Objects always start with a 3 letter prefix
 2. Columns always start with 4 letter abbreviation of the functional part of the table name
 3. Dependent PL/SQL objects like parameters and variables always start with a 2 letter abbreviation like pi, pm, po, pc for IN, IN OUT(modify), OUT and Cursor Parameters or co for Constants.
 
This convention helps with automatic checks as the position of the first or second underscore can always be determined automatically and on one side combined with the actual object characteristics and on the other side with the aforementioned abbreviation dictionary.
## Underscores
SQL and PL/SQL are defined as Case Insensitive languages. Research has found that readability improves the closer the program text is to everyday writing. The Underscore is visually close to the space character, so should be preferred as a separator. Ideally each statement should be written like a sentence. That would mean INITCAP for all reserved words and Lowercase for all other identifiers.
## Combinations
There can be good reasons to use more than one abbreviation. For instance to separate Tables from Views, important when analyzing an existing programm, to group tables belonging to one Projekt together or to reference a parent table in the child table name by its abbreviation (before 128 bytes). Because several abbreviations after another get unreadable quickly one should avoid more than two abbreviations in a name. As the abbreviations should be used for generic groupings putting them in front as prefixes has the advantage of showing these groupings simply by ordering alfabetically.



