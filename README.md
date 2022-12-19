# **SuperMart-Grocery-Sales**
Data Analysis or sometimes referred to as exploratory data analysis (EDA) is one of the core components of data science. It is also the part on which data scientists, data engineers and data analysts spend their majority of the time which makes it extremely important in the field of data science
For purpose of illustration the [Super Market Sales](https://drive.google.com/file/d/1sgsX_WDBnOYsLMVMNvr6_hlV5cppmlfJ/view?usp=sharing) dataset has been taken.
## Dataset Overview
- Understanding shape of the data. The data has 1000 rows and 20 columns.
- Data description. 
1. Unit Price has a range of 10-99, with mean price of 55.
2. Avg qty ordered per transaction is 6.5, the range being 1-14.
3. The invoice total mean is 323, and the range is 10-1042.
4. Gross Margin is constant at 4.76%.
5. The mean rating is 7.46/10.
- We will use info() to check null values, and data types of all variables.

| Data types| Number of columns|
|------|------|
| Object| 10|
| Float64| 9|
| Int| 1|
- Using isnull() we find out number of null values in each column. There are 7 columns having null values. Among all the Branch columns has the maximum null values - 194 and Payment columns has minimum null values - 21.
## Data Wrangling
### Data imputation
- Before moving further we made a copy of data using copy().
- Imputing null values for 'Rating' column. Here we don't have any values so we shall take the mean value of rating.
- Same as above for all the quantitative columns having null values we shall impute null values with mean value of the columns.
- Qualitative columns also have null values but here we can't take mean value, so we shall take the mode of the value(most repeated value) for imputing null values.
- There are 4 qualitative columns having null values. Imputing all of them one by one will be a time consuming specificaly in that case where number of columns are huge. For this problem we shall write a function. 
``` 
    def impute_mode(col):
        print("The mode of this field is :  " + df1[col].mode()[0])
        df1[col] =  df1[col].fillna(df1[col].mode()[0])
        print("Mode value imputed");
```

- The 'Branch' column has null values but here we can't take mode of values because there are 3 different branches loacted in 3 different cities. 
- Again a we shall require a function for imputation. 
``` 
  for i in range(len(df1['Branch'])):
  if pd.isna(df1['Branch'][i]) == True:
    if (df1['City'][i].strip()=="Yangon"):
      df1['Branch'][i]=='A'
      print('NA imputed as branch A')
    elif df1['City'][i].strip()=='Naypyitaw':
      df1['Branch'][i]=='C'
      print('NA imputed as branch C')
    elif df1['City'][i].strip()=='Mandalay':
      df1['Branch'][i]=='B'
      print('NA imputed as branch B')
    else:
      print('City not available')
 ```
 
 - After null% verification it is confirmed that all null values have been imputed.
 
 
