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
 
 # **Univariate Analysis(Quantative)**
 It is the most simplest form of statistical analysis. The key fact is that only one variable is involved. 
 Here we shall use voilin plot and stripplot for analysis. For that purpose we need to write a function. 
  
 ``` 
  # Defining function for Univariate Analysis of Quantitative Variables

def univariate_quant(col,hue=None):

    fig, axes=plt.subplots(nrows =2,ncols=1,figsize=(20,12))        # Defining 2 subplots, changing fig size
    axes[1].set_title( "Boxenplot of "+ col , size =14)             # Chart title for Subplot 1
    sns.stripplot(df1[col],ax=axes[1], color="#4CB391")             # Distplot in subplot 1


    axes[0].set_title("Violinplot for  " + col )                    #  Title for Subplot 2
    sns.violinplot(df1[col],ax=axes[0], color="grey")               # Violinplot in Subplot 2
    
                      
    plt.tight_layout()
    fig.savefig("univariate_"+col+".png")
  ```
  
### 1. Unit price 
![image](https://user-images.githubusercontent.com/90236224/208816312-8b4eabc2-c428-4a97-b120-ea8ac860ed0a.png)
- Unit price doesn't follow a normal distribution
- it is more or less similar frequency for the range 20-80
  
### 2. Quantity
![image](https://user-images.githubusercontent.com/90236224/208816498-82072637-8cee-4208-8403-c261b3615b48.png)
- The distribution is a relatively flatter one. The range 1-13 is the densest
- Mean, and peak coincide at 7, meaning 7 is the most frequent order size.

### 3. Total
![image](https://user-images.githubusercontent.com/90236224/208816657-803642f5-f00e-435e-b95b-af01fa57c84a.png)
- Total sales is densest between 0 to 500 which means customers prefer small amount purchase.

### 4. Gross income
![image](https://user-images.githubusercontent.com/90236224/208817252-a77dfb73-7ec6-46d0-9caa-941d8f7bf9b9.png)
- It has right skewed distribution, with most values lies under 500 range.

### 5. Rating
![image](https://user-images.githubusercontent.com/90236224/208817385-57119457-0da5-475f-a418-5b3e7b3e3e17.png)
-  Ratings have a similar distribution with a mean of around 7.5 ratings
- There are much lesser ratings in the 3-5 range, which is good

# **Univariate analysis (Qualitative)

### 1. Across city
![image](https://user-images.githubusercontent.com/90236224/208817645-ee512894-3ad3-4aba-9947-5e876c5ea586.png)
- All cities have an almost equal number of transactions. In other words, the footprint of the supermarket is similar across cities.

### 2. Payment
![image](https://user-images.githubusercontent.com/90236224/208820986-85bf067d-4e13-4cf9-8923-951942f42d6d.png)
- E-Wallet is the most preferred payment mode while Credit card is the least

### 3. Gender
![image](https://user-images.githubusercontent.com/90236224/208821047-c338ba54-71db-429c-85cd-1ec80aa58ef5.png)
- Females transact more as compared to males. There can be multiple reasons for this which can be a part of the further analysis -
- Is the supermarket known for female-oriented products?
- Does the region, in general, have more females than males?
- It might be a surprising insight for the supermarket, and a particular factor might be causing it - Store aesthetics, offers on a select
  catalogue etc.
  
### Product Line
![image](https://user-images.githubusercontent.com/90236224/208821172-1173dbc8-e8d0-4e5d-a388-659a6542d47c.png)
- There are two types of product lines because there is a huge gap in # transactions among the top 3 and bottom 3 product lines.
1. Health & Beauty, Sports, Fashion constitute the majority of the transactions and are high selling product lines for the
supermarket.
2. The other 3 product lines are not selling as much.
3. Health and Beauty is a top-selling product line with a healthy margin.
The possible reasons for this might be the following -
- There is differentiation in focus by the supermarket - there are more products on sale from the top 3 as compared to the
bottom 3
- There are special offers on the top 3 which are driving the sales
- The prices and quality of the top 3 are more competitive than the rest
- The management is more focused and concerned with the growth of the top 3 rather than others ( Possibly due to low
margins )

### Customer Members
![image](https://user-images.githubusercontent.com/90236224/208821250-adf6e669-6232-4dd9-b666-2536ec69ad22.png)
- 2/3rd of the transactions are done by Normal Members and only 1/3rd by Members







  
 
