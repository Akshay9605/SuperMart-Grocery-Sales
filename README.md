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

### Geographical location
[Geo map](https://goo.gl/maps/EWAAQeQLAebCYPfm8)
![image](https://user-images.githubusercontent.com/90236224/208824428-a088fe46-1e5f-4ccb-aa81-d01ae72d118e.png)

- The cities are distributed across lengths of Myanmar and are not close to each other
- Yangon is the only coastal city where the chain is present. Other ones are located in the middle of Myanmar
- The company can expand in the North and East of Myanmar as there is no presence in those areas
- A big coastal city Sittwe is also an option for later expansion

# **Bivariate Analysis**
Bivariate analysis is one of the statistical analysis where two variables are observed. One variable here is dependent while the other is independent. These variables are usually denoted by X and Y. So, here we analyse the changes occured between the two variables and to what extent. 

### 1. Heatmap
The purpose of Heatmap here is to look for any significant correlations among continuous variables in our data.
![image](https://user-images.githubusercontent.com/90236224/208824854-3119f89d-a236-4537-be2b-1bfda54489d0.png)

- We see a high correlation in almost all sales-related fields because they are derived from each other.

Four important metrics can provide maximum insights into this sales data -
1. Sales
2. AOV - Average Order Value
3. Mean Order Quantity
4. Ratings
It would be worthwhile to plot these against multiple categorical variables to see if those categories impact these four metrics.
It can be a great way to determine what's working for the chain and what is not.

We shall write a function for bivariate analysis

```
    # Defining function for Univariate Analysis of Quantitative Variables

def grouped_analysis(col,hue=None):
    
    plt.figure(figsize=(20,10))
    
    def custom_fmt(x):                                                     # Custom format function to show values in pie chart
        return '{:.0f}%\n({:.0f})'.format(x, sales_grouped['Total'].sum()*x/100)  # It is used in autopct parameter in pie chart

    
    sales_grouped= df1[[col,'Total']].groupby(col).sum()                # Sales grouped by col
    mean_ratings = df1[[col,'Rating']].groupby(col).mean()              # Avg ratings grouped by col
    aov  = df1[[col,'Total']].groupby(col).mean()                       # AOV by col
    mean_units_qty = df1[[col,'Quantity']].groupby(col).mean()          # Mean order qty by col

    fig, axes=plt.subplots(nrows =2,ncols=2,figsize=(20,12))                      # Defining 4 subplots, changing fig size
    axes[0,0].set_title("Sales by " + col , size = 25)                            # Chart title for Subplot 1
    axes[0,0].set_xticklabels(axes[0,0].get_xticklabels(), fontsize=20)
   
    _=axes[0,0].pie(sales_grouped['Total'], labels = sales_grouped.index, autopct= custom_fmt,textprops={'fontsize': 14})


    axes[0,1].set_title("AOV by "  + col,size = 25 )                              #  Title for Subplot 2
    axes[0,1].set_xticklabels(axes[0,0].get_xticklabels(), fontsize=20)    
    axes[0,1].set_xlabel( axes[0,0].get_xticklabels(),fontsize=20)
    axes[0,1].set_ylabel( axes[0,0].get_yticklabels(),fontsize=20)
    g=sns.barplot(x=aov.index, y='Total', color="#f7a516",data=aov,ax=axes[0,1]) 
    g.set_xticklabels(
    labels=aov.index, rotation=45)                                                # Rotating lables so that they dont overlap
    
    
    axes[1,0].set_title("Mean Ratings by " + col,size = 25 )                      # Title for Subplot 3
    axes[1,0].set_xlabel( axes[0,0].get_xticklabels(),fontsize=20)
    axes[1,0].set_ylabel( axes[0,0].get_yticklabels(),fontsize=20)
    sns.barplot(y=mean_ratings.index, x='Rating', color="#305cb0",data=mean_ratings,ax=axes[1,0],orient='h')

    
    axes[1,1].set_title("Mean Units Qty by " + col,size = 25 )                    # Title for Subplot 4
    axes[1,1].set_xlabel( axes[0,0].get_xticklabels(),fontsize=20)
    axes[1,1].set_ylabel( axes[0,0].get_yticklabels(),fontsize=20)
    sns.barplot(y=mean_units_qty.index, x='Quantity', color="#712f80",data=mean_units_qty,ax=axes[1,1],orient='h')

    plt.tight_layout()
    fig.savefig("grouped_analysis"+col+".png")
```

### 1. Across City
![image](https://user-images.githubusercontent.com/90236224/208825586-19260327-9977-4108-a83a-3b2f8dc8dfd7.png)
- Although the number of transactions was similar to what we saw earlier in the Univariate analysis, Mandalay city has
  significantly more sales at 39%
- Yangon sales are least at 28%
- The difference in sales is essentially driven by the differences in AOV, which is maximum for Mandalay and low for Yangon
- Further, we can say that the higher AOV is driven by a higher mean quantity, as seen in the Right bottom chart
- Ratings also reflect a similar story, with the highest ratings for Mandalay and least for Yangon
- The above factors point to a significant scope of improvement in Yangon for the supermarket chain

### 2. Customer type
![image](https://user-images.githubusercontent.com/90236224/208825674-e8ca9eb1-6899-4132-8669-f97b0c1f6265.png)
- Members are contributing to 43% of sales, driven by a high AOV of 500 a compared to 300 of a normal customer
- The difference in Average order quantity drives the difference in AOV
- Ratings are similar among members and Normal customers

### 3. Gender
![image](https://user-images.githubusercontent.com/90236224/208825734-74e58412-d5d9-400e-9fc4-eedf994d9df4.png)
- As expected after Univariate analysis, females are driving more sales at 55%
- The AOV and average order quantity are also significantly higher for females
- The ratings are not impacted by Gender

### 4. Product line
![image](https://user-images.githubusercontent.com/90236224/208825836-34d4e3b2-c4f8-4216-83b7-a8594fe70a8f.png)
- Health and Beauty is the most prominent segment with 30% of sales
- Three bottom segments - Food & Beverages, Electronics, and Home & lifestyle together make 30% of the sales
- Food & Beverages have the maximum AOV among all categories. Home and lifestyle also have a relatively higher AOV
- Customers love these two segments as the ratings are also highest for these two segments
- If the company could drive more transactions in the above two segments, it would boost the sales significantly due to their
  higher AOV
  
### 5. Payment
![image](https://user-images.githubusercontent.com/90236224/208825959-36015648-a1f0-4c8a-9887-4adbb65b1f3f.png)
- Payment mode doesn't seem to impact any metric as modes have equal distribution in sales, ratings etc.
- AOV for Credit cards is relatively higher, but the difference is not significant to prove a hypothesis

# **Time Series Analysis**
Time is significant variable of the data. Time series analysis is a specific way of analyzing a sequence of data points collected over an interval of time. In time series analysis, analysts record data points at consistent intervals over a set period of time rather than just recording the data points intermittently or randomly.

### Reason to use time series.
- Cyclicity in sales can be visualised
- Seasonal sales patterns can be identified
- Impact of various events ( Festivals, Market collapse, Natural disasters etc. ) can be identified
- New strategies can be devised based on the insights to lift sales during specific times
- Ideal promotional times can be identified

Our first step is to convert date column into datetime. 
We have added some more columns to the data such as: month, day, weekday, hour and minute.
We shall write a function. 

``` 
    # Defining function for Univariate Analysis of Quantitative Variables

def timeseries_analysis(col,hue=None):
    
    plt.figure(figsize=(20,10))
    
    sales_grouped= df1[[col,'Total']].groupby(col).sum()              # Sales grouped by col
    mean_ratings = df1[[col,'Rating']].groupby(col).mean()            # Avg ratings grouped by col
    aov  = df1[[col,'Total']].groupby(col).mean()                      # AOV by col
    mean_units_qty = df1[[col,'Quantity']].groupby(col).mean()         # Mean order qty by col


    fig, axes=plt.subplots(nrows =2,ncols=2,figsize=(20,12))                      # Defining 4 subplots, changing fig size
     
    
    axes[0,0].set_title("Sales by " + col , size = 25)                            # Chart titl for Subplot 1
    sns.lineplot(x=sales_grouped.index , y= sales_grouped['Total'], data=sales_grouped, ax=axes[0,0])


    axes[0,1].set_title("AOV by "  + col,size = 25 )                              #  Title for Subplot 2
    axes[0,1].set_xticklabels(axes[0,1].get_xticklabels(), fontsize=20)    
    axes[0,1].set_xlabel( axes[0,1].get_xticklabels(),fontsize=20)
    axes[0,1].set_ylabel( axes[0,1].get_yticklabels(),fontsize=20)
    sns.barplot(x=aov.index, y='Total', color="#f7a516",data=aov,ax=axes[0,1])    
    
    
    axes[1,0].set_title("Mean Ratings by " + col,size = 25 )                      # Title for Subplot 3
    axes[1,0].set_xlabel( axes[1,0].get_xticklabels(),fontsize=20)
    axes[1,0].set_ylabel( axes[1,0].get_yticklabels(),fontsize=20)
    sns.barplot(y=mean_ratings.index, x='Rating', color="#305cb0",data=mean_ratings,ax=axes[1,0],orient='h')

    
    axes[1,1].set_title("Mean Units Qty by " + col,size = 25 )                    # Title for Subplot 4
    axes[1,1].set_xlabel( axes[1,1].get_xticklabels(),fontsize=20)
    axes[1,1].set_ylabel( axes[1,1].get_yticklabels(),fontsize=20)
    sns.barplot(y=mean_units_qty.index, x='Quantity', color="#712f80",data=mean_units_qty,ax=axes[1,1],orient='h')

    plt.tight_layout()
    fig.savefig("timeseries_analysis"+col+".png")
```

### 1. Hour
![image](https://user-images.githubusercontent.com/90236224/208827339-868bfe2c-1239-42a6-a3dc-15ec6621c7d2.png)
- Sales peak sharply at 7 PM
- 4 PM-6 PM is a dead period with meagre sales
- Stores see moderate sales from 10 AM to 3 PM
- Ratings, Order quantity logically should not be related to the time of the day. Hence any variation should be attributed to a
  random phenomenon
  
### 2. Month
![image](https://user-images.githubusercontent.com/90236224/208827418-23bfde23-21a4-4cce-b809-5c03862a6636.png)
- February is a peak sales month where sales go beyond 50k+. For comparison, most of the other months accumulate less than
  35k revenue
- There must be a reason for this spike in February. It can either be a festival in the region, a season that's driving sales of a
  particular segment, or a promotional campaign that has worked well
- December also sees a significant spike, although not as steep as February but still significant
- Sales are lower in Apr-Nov, dipping close to 25k in July

### 3. Month on month sales, members and non-members
![image](https://user-images.githubusercontent.com/90236224/208827610-5621e194-fe3d-4829-bc32-c64179f64f25.png)
- Members always have significantly more AOV than normal customers
- Members AOV has lesser fluctuations than Non-members

### 4. Customers across city
For the purpose of finding number of customers across the city and sales per customers by cities we shall write a function.
```
plt.figure(figsize=(15,8))						
						
customer_city = df1[['City', 'CustomerID']].groupby(['City']).nunique()						
sales_city= df1[["City",'Total']].groupby("City").sum()						
sales_per_cx = sales_city["Total"]/customer_city["CustomerID"]						
						
fig, ax=plt.subplots(nrows =1,ncols=2,figsize=(20,12)) # Defining 4 subplots, changing fig size						
						
sns.barplot(x=customer_city.index, y='CustomerID', color="#f7a516",data=customer_city,ax=ax[0])						
_=ax[0].set_title("Customer Count by Cities", size = 25) # Chart titl for Subplot 1						
						
						
sns.barplot(x=customer_city.index, y=sales_per_cx.values, color="#f7a516",ax=ax[1])						
_=ax[1].set_title("Sales per Customer by Cities", size = 25) # Chart title for Subplot 2	
``` 

![image](https://user-images.githubusercontent.com/90236224/208827956-01b5ef8c-2623-4bc1-83f0-bf743dccd2a0.png)
- Mandalay has the least unique customers (90), while Yangon has the most (200+)
- Sales per customer metric have an opposite trend where each customer spends maximum in Mandalay ( 1600)

### 5. Members and non-members
Now we shall understand the relationship between members and non-members on the basis of 4 metrics.
1. Sales by city/customer type
2. Average order values by city/customer type
3. Mean ratings by city/customer type
4. Mean order quantity by city/customer type

Here again we shall write a function.

```
sales_grouped= df1[["City","Customer type",'Total']].groupby(["City","Customer type"], as_index = False).sum()												
mean_ratings = df1[["City","Customer type",'Rating']].groupby(["City","Customer type"], as_index = False).mean()												
aov = df1[["City","Customer type",'Total']].groupby(["City","Customer type"], as_index = False).mean()												
mean_units_qty = df1[["City",'Customer type','Quantity']].groupby(["City","Customer type"], as_index = False).mean()												
fig,axes= plt.subplots(nrows =2,ncols=2,figsize=(20,12))												
_=sns.barplot(x=sales_grouped["City"], y='Total',data=sales_grouped,hue = 'Customer type', ax = axes[0,0])												
axes[0,0].set_xticklabels(axes[0,0].get_xticklabels(), fontsize=20)												
axes[0,0].set_title("Sales by City/Cutomer Type " , size = 25)												
_=sns.barplot(x=aov["City"], y='Total',data=aov,hue = 'Customer type', ax = axes[0,1])												
axes[0,1].set_xticklabels(axes[0,1].get_xticklabels(), fontsize=20)												
axes[0,1].set_title("AOV by City/Cutomer Type " , size = 25)												
_=sns.barplot(x=mean_ratings["City"], y='Rating',data=mean_ratings,hue = 'Customer type', ax = axes[1,0])												
axes[1,0].set_xticklabels(axes[1,0].get_xticklabels(), fontsize=20)												
axes[1,0].set_title("Mean Ratings by City/Cutomer Type " , size = 25)												
_=sns.barplot(x=mean_units_qty["City"], y='Quantity',data=mean_units_qty,hue = 'Customer type', ax = axes[1,1])												
axes[1,1].set_xticklabels(axes[1,1].get_xticklabels(), fontsize=20)												
axes[1,1].set_title("Mean Order Qty by City/Cutomer Type " , size = 25)												
												
plt.tight_layout()												
```

![image](https://user-images.githubusercontent.com/90236224/208828487-715359be-39c3-4ace-9104-dd86cafa61ff.png)
- Normal customers have more sales in all cities. But that may be because Non Members are higher in the count, as we've seen
  earlier
- Members consistently have higher AOV than non-members
- The higher-order quantity drives the AOV

**Here we have completed our analysis. From the data cleaning, exploratory data analysis, univariate analysis to bivariate analysis. We have also generated insights which is important for the managment for decision taking.**
Now at the end of this project we will conclude by putting our final insights. What we have understood from the data analysis and recommendation what should be implemented for improving business operations, sales and profit.

# **Conclusion**



												
												














  
 
