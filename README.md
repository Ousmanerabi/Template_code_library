### Step 1: Install packages
```r
install.packages(c('dplyr', 'readxl', 'tidyverse', 'janitor', 'writexl'))
```
`dplyr`: vvvv

`readxl`: vv

`janitor`: vv

`tidyverse`: vv

`writexl`:vvv

### Step 2: Load the packages already installed
```r
library(dplyr)
library(readxl)
library(tidyverse)
library(janitor)
library(writexl)

```
### Step 3: Import data

#### Step 3.1 : Setup the directory
```r
setwd()
```
` setwd`: file path
#### Step 3.2 : Read a single file:
```r
raw_data = read.csv('file path/filename.csv')
raw_data = read.csv2('file path/filename.csv')
raw_data = read_excel('file path/filename.xlsx')
raw_data = read_excel('file path/filename.xls') 
```
`read_csv()`: is special cases of the more general read_delim(). They're useful for reading the most common types of flat file data, comma separated values and tab separated values, 
 respectively. 

`read_csv2()`: uses ⁠;⁠ for the field separator and ⁠,⁠ for the decimal point. This format is common in some European countries

`read_excel()`: calls excel_format() to determine if path is xls or xlsx, based on the file extension and the file itself, in that order. 
 Use 'read_xls()' and 'read_xlsx()`: directly if you know better and want to prevent such guessing.
#### Step 3.3 : Read multiple files and bind it into a single file
```r



```
### Step 4: View the data (description of data)
```r



```
### Step 5: Select and rename key variables
```r



```
### Step 6: Compute new variables
```r



```
### Step 7: Transform data element into appropriate data type
```r



```
### Step 8: Save the transform data in excel/csv
```r



```
### Step 9: Aggregate data element monthly and yearly at the appropriate admnin level

#### Step 9.1: Aggreate data element monthly at the appropriate admnin level and save it
```r



```
#### Step 9.2: Aggreate data element yearly at the appropriate admnin level and save it
```r



```


