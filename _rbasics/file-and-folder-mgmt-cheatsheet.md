# R Cheatsheet - File and Folder Management

https://cloud.r-project.org/web/packages/fs/vignettes/function-comparisons.html
https://www.rdocumentation.org/packages/base/versions/3.6.2/topics/files
https://theautomatic.net/2018/07/11/manipulate-files-r/

Order of operations
1. Does path exist
2. Overwrite
3. Recursive

## 1 Basics

| Action 	| Function 	| Description 	|
|:------	|:--------	|:-----------	|
|get working directory       	|**```getwd()```**          	|returns the current working directory             	|
|set working directory        	|**```setwd()```**          	|changes the working directory             	|
|set file path        	|**```file.path()```**          	|sets the path             	|
|        	|          	|             	|

## 2 Create and Copy

| Action 	| Function 	| Description 	|
|:------	|:--------	|:-----------	|
|create folder       	|**```dir.create(file.path("path"))```**          	|creates a folder in the working directory if ```file.path()``` left blank or other location if **path** specified             	|
|create file       |**```file.create(file.path("path"))```**          	|creates a file in the working directory  if ```file.path()``` left blank or other location if **path** specified           	|
|create multiple folders        |**```sapply(file.path(paste0("path","folder",1:10)),dir.create)```**          	|creates 10 empty folders in the working directory called **folder1**, **folder2** etc. if ```file.path()``` left blank or other location if **path** specified            	|
|create multiple files        |**```sapply(file.path(paste0("path","file",1:10,".txt")),file.create)```**          	|creates 10 empty **.txt** files in the working directory called **file1.txt**, **file2.txt** etc. if ```file.path()``` left blank or other location if **path** specified|


|copy folder       	|**```dir.create("new-path"); file.copy("path", "new-path", recursive=TRUE)```**|folders and the contents of those folders cannot be copied unless they first exist.  to copy an existing folder **path** including the content and sub-folders, R first creates **new-path** then copies the **path** folder.              	|
|copy file        |**```file.copy()```**          	|creates a copy of the file in the specified folder. ```file.copy("current-file.txt", "new-path")``` will make a copy of **current-file.txt** and move it to **new-path** returning TRUE if successful.             	|

## 1 Template

| Action 	| Function 	| Description 	|
|:------	|:--------	|:-----------	|
|action1       	|**```function1()```**          	|desc1             	|
|action2        |**```function2()```**          	|desc2             	|
|action3        |**```function3()```**          	|desc3             	|
|action4        |**```function4()```**          	|desc4             	|

### Terminology

#### 1 Top-Level Folder/Directory

The root directory

## Resources
* [R: HOW TO CREATE, DELETE, MOVE, AND MORE WITH FILES](https://theautomatic.net/2018/07/11/manipulate-files-r/)
