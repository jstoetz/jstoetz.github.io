---
layout: collection
title: file naming conventions and organization
---

### File Types

#### Data Files
Data files tend to come in many different forms - .csv, .txt, .xls etc.  Sometimes you can't control the type of file you are working with.  Sometimes the data is just *given* to you and you have to figure it all out.
#### R files
#### functions
```
# good
fn.all.weekdays
# bad
get.all.the.weekdays.from.now.until.later
```
#### Templates

#### data frames

### File Names
#### Purpose
The goal of any good naming system is to convey *content*. If you can look at a file name and immediately associate it with a specific project and understand the content of the file, then you have achieved your goal.
#### Format
##### R files.
File names should be meaningful and end in .R.

##### Underscores
All files - regardless of type - should separate relevant names by underscores.  Avoid using special characters in file names - stick only with numbers, letters, and underscores.

```
# Good
fit_models.R
utility_functions.R

# Bad
fit models.R
foo.r
stuff.r
```

##### Lower Case
All names shall be in lower case.

```
# Good
p12_blog1_data_section_1.csv
# Bad
P13.FILE.FOR.THE.THINGY.xls
```

##### Ordering
If files should be run in a particular order, prefix them with numbers. If it seems likely you’ll have more than 10 files, left pad with zero:
          ```
          00_download.R
          01_explore.R
          ...
          09_model.R
          10_visualize.R
          ```

### External Organization
#### Folder Structure

### Internal Organization

#### Front Matter
##### Who Wrote the Code
##### Contact Information:  Name/Email/Social
##### Date Started

#### Dividers
##### Heading Dividers
##### Main Dividers
##### Comments

### Internal Structure
