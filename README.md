# project-adhoc

### [1]PivotTable
   - Clean data, added month column

  ##### Total Sales per month of each customer
          - pivot table
          - index = ["name","username"]
          - columns = ["month"]  
          - values = sum of ["transaction_value"]

  ##### Count of customer purchase per month
          - Yield count with groupby where index='name' and column='month'
 
 
### [2]ItemBreakdown
   1. Split rows to transform data into a granularity of one “line item” per row
   2. Converted string of dates in 'transaction_date' into datetime value to easily manipulate naming conventions
   3. `brand_split` dataframe
       - split 'transaction_items' to get quantity of each item in integer value 
   4. `new_df` dataframe is merging complete dataframe (tdf) with subdataframe (brand_split) to match other values/

  ##### Total count of items sold per month
          - pivot table
          - index = ["item"]
          - columns = ["month"]  
          - values = sum of ["quantity"]

  ##### Breakdown of items sold per customer
          - groupby where index=['name','username',"item","brand"] and column=['quantity']
          - yields the sum items purchased by each customer

  ##### Total no. of items sold by brand
          - groupby where index=["brand","item"] and column=['quantity']
          - yields total number of quantities sold for each item 


### [3]Category
   1. Created a table that shows 1 if customer purchased in a certain month and 0 if no purchases were made
      - to make comparison and referencing easier
   2. Only referenced the table so customers in each category per month can be accessed.

  
  ##### Engaged, New, Inactive
      `sum6 = df6[[1,2,3,4,5,6]].sum(axis=1)`
      - gets the sum of columns to be used later for referencing
  
  ##### Engaged
      `eng6 = df6[sum6 == 6]`   OR    `eng6 = df6[df6[[1]].sum(axis=1) == 6]`
      - if `sum6` == 6, then based on the dataframe, a value of 1 is present in all months from January to June
  
  ##### New
      `sum5 = df6[[1,2,3,4,5]].sum(axis=1)`
      `new6 = (df6[sum5 == 0] [df6[sum5 == 0][6]==1])`
      - if `sum5` == 0, then no purchases were made from January(1) to May(5)
      - first purchase was made on June(6), hence, [df6[sum5 == 0][6]==1])
      - thus, customer is new
  
  ##### Inactive
      `sum6 = df6[[1,2,3,4,5,6]].sum(axis=1)`
      `inac6 = (df6[sum6 != 0] [df6[sum6 != 0][6]==0])`
      - if `sum6` != 0, then a previous purchase has been made
      - then, if June(6)==0 among the customers in the dataframe with previous purchases, customer is inactive in June
  
  ##### Repeater
      `rep6 = df6[df6[[5,6]].sum(axis=1) == 2]`
      - if May(5) + June(6) == 2, then based on the dataframe, purchases were made in both months
      - thus, customer 
  

### [4]SalesBreakdown
     
  ##### Unit Price
      1. Split 'transaction_items' by ';' to get number of orders per customer
      2. Extracted customers with only 1 order with `count`
      3. Extracted orders with only 1 quantity
      4. Removed duplicates to get table with only the items
      5. Modified final unit_price table

  ##### Sales Breakdown
      1. Used item sold breakdown table from [2]ItemBreakdown
      2. Converted columns into strings for referencing
      3. Merged `Unit Price` and `Items Sold` for multiplication of values
      4. Added columns for multiplied values yielding sales of each item per month.
  
