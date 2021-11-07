---
title: "Importing Data from Excel to GAMS"
categories:
  - blog
  - GAMS
tags:
  - Software
  - Optimization
  - GAMS
  - Excel
---
There are various methods to import data from excel to GAMS. GDXXRW is a similar to database tool that serves as an intermediate tool to transfer data in both directions. In this post I provide some simple and clean lines of code to utilize this useful tool in order to import data from excel to GAMS. I explain the code in a procedural manner so you can keep it in mind the steps you should carry on to import your desired data.

# Step 1: Define sets and parameters you wish to import
Remember that this step is neccessary! If you do not define the sets and the parameters, compiler will result in error 140 (Unknown Symbol). 
```gams
set r, c, d;
parameter p, h, t;
```
# Step 2: Create a text file containing data import addresses, types and names from within GAMS IDE
It is better to create a text file from within GAMS IDE to determine all your desired data import tasks in one place. In step 3 we will refer to this text file and command GAMS to import data based on what's written there. The syntax to create a text is completly straight forward. Use `$onecho > 'filename.txt'` and `$offecho` to set the start and end of the text file, respectively.
```gams
$onecho > tasks.txt
dset=r            rdim=1
dset=c            cdim=1
dset=d     rng=g2 cdim=1
par=p rng=inputcase
par=h rng=a1 rdim=1 cdim=1
par=t rng=h7 rdim=0
$offecho
```
Here you see that I created tasks.txt file and set my command between the onecho and offecho lines. Let's burst into what data I am going to import.
The first line `dset=r   rdim=1` means that I need to import a **set** which I define name `r` from my first sheet in excel file. `rdim=1` specifies that your set elements are stored in **one column** (multiple rows) and not in multiple columns and one row.
`dset=c   cdim=1` is similar to the previous code but it is used when your set elements are store in **one row** and multiple columns. Remember that by default the pointer will start reading data from `Sheet1!A1` address in excel workbook, Unless you define the starting (or arbitrarily the ending) range.
`dset=d   rng=g2 cdim=1` will import set `d` by starting from cell `G2` within first sheet of the workbook and similarly the dimension is column based and equal to one. Here you mentioned that I didn't define any ending address for the pointer. In this case the pointer will stop reading whenever faces **two consecutive blank cells**. If you want to specify the ending address, simply use `starting address:ending address` syntax.
`par=p rng=inputcase` means that I want to import a **parameter** which I name it `p` in my GDX file and the range the pointer should lookup is named `inputcase` in my excel file. Note that you could define names in excel by simply selecting a range of cells, right click on it and choose `Define Name`.
But if you wish not to use defined names, you could specify the range with `rng=address` command and also remember to set the dimension of the underlying sets. For example I imported parater `h` by this method.
`par=t rng=h7 rdim=0` means that I want to import a zero dimensional parameter which is the same as a scalar. Just set the address of the unique cell and the row or column dimension to zero.

# Step 3: Say to GAMS that you want to import data regarding the text file settings from excel to GDX file
The command is like this:
```gams
$call gdxxrw i="Excel file path" o="GDX file path" @textfile.txt
```
We use `$call gdxxrw` command to call gdxxrw tool which is our tool to import data from excel to GDX. Then you should specify the excel file path by `i="Excel file path"` command. After that Determine the GDX file path that you wish your data to be stored in there by `o="GDX file path"` command. Finally tell GAMS that the instructions to import data are stored in a text file named `textfile.txt`. For example in step 2, I used the name `tasks.txt` name for my text file name.

# Step 4: Import data from GDX file to GAMS runtime
First open the GDX file:
```gams
$gdxin "GDX file path"
```
Second, load the data stored in the GDX file:
```gams
$load r c d
$load p h t
```
Finally, close the GDX file:
```gams
$gdxin
```
So the code will be like this:
```gams
set r, c;
parameter p;
$onecho > tasks.txt
dset=r            rdim=1
dset=c            cdim=1
dset=d     rng=g2 cdim=1
par=p rng=inputcase
par=h rng=a1 rdim=1 cdim=1
par=t rng=h7 rdim=0
$offecho
$call gdxxrw i="Excel file path" o="GDX file path" @textfile.txt
$gdxin "GDX file path"
$load r c d
$load p h t
$gdxin
```
Enjoy coding :-)
