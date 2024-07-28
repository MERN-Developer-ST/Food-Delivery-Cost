# Food-Delivery-Cost-Analysis:-
This project provides an in-depth analysis of food delivery costs using a dataset containing various financial aspects of food delivery orders. The analysis includes data cleaning, transformation, and visualization to understand the distribution and impact of different cost components on the overall profit. The code is written in Python and leverages libraries like pandas and matplotlib for data manipulation and visualization.

  # Project Steps:-

1. Load and Inspect Data:
2. Convert Date Columns to Datetime:
3. Clean and Transform Discounts and Offers:
4. Calculate Costs and Profit:
5. Summarize Cost Distribution:
6. Visualize Data:

# Description Of Steps:-

1. Load and Inspect Data:
 
 --> The first step involves loading the dataset into a pandas DataFrame and inspecting its structure to understand the data.

 -->
      import pandas as pd
      df = pd.read_csv("food delivery costs.csv")
      df.head()
      df.info()
   

2. Convert Date Columns to Datetime:
   
--> Next, we convert the order and delivery date columns to datetime objects to facilitate date and time operations.

-->
      df["Order Date and Time"] = pd.to_datetime(df['Order Date and Time'])
      df["Delivery Date and Time"] = pd.to_datetime(df["Delivery Date and Time"])
      df.info()


3. Clean and Transform Discounts and Offers:

--> This step involves cleaning the "Discounts and Offers" column by extracting and converting percentage values to float. If the value is less than 15, it is considered as a percentage and converted to a monetary amount based on the "Order Value".

--> **Code**  
    def extract(value):
    a = str(value).split(" ")
    return a[0]

   df["Discounts and Offers"] = df["Discounts and Offers"].apply(extract)
   
   def removep(value):
       if "%" in value:
           a = value.replace("%","")
           return float(a)
       else:
           return float(value)

   df["Discounts and Offers"] = df["Discounts and Offers"].apply(removep)
   df.loc[(df["Discounts and Offers"] <= 15),"Discounts and Offers" ] = (df["Discounts and Offers"]/100) * df["Order Value"]
   df["Discounts and Offers"] = df["Discounts and Offers"].fillna(0)   
   

4. Calculate Costs and Profit:

--> In this step, we calculate the total costs incurred for each order and then determine the profit by subtracting these costs from the commission fee.

-->
   df["Costs"] = df["Delivery Fee"] + df['Discounts and Offers'] + df["Payment Processing Fee"]
   df["Profit"] = df["Commission Fee"] - df['Costs']

   
5. Summarize Cost Distribution:

--> We aggregate the cost components to understand their distribution across the dataset.

-->
   cost_dist = df[["Delivery Fee", "Payment Processing Fee", "Discounts and Offers"]].sum()
   

6. Visualize Data:

--> Finally, we create visualizations to represent the cost distribution and overall financial summary:
    - A pie chart for cost distribution.
    - A bar chart comparing commission fees, total costs, and profit.
    - A histogram to visualize the distribution of profit.


   
-->
   import matplotlib.pyplot as plt

   # Pie Chart for Cost Distribution
   plt.figure(figsize=(3,3))
   plt.pie(cost_dist, labels=cost_dist.index, autopct="%1.2f%%")
   plt.show()

   # Bar Chart for Commission, Costs, and Profit
   abc = df[["Commission Fee", "Costs","Profit"]].sum()
   plt.figure(figsize=(3,3))
   plt.bar(abc.index, abc)
   plt.xticks(rotation=90)
   plt.show()

   # Histogram of Profit
   plt.hist(df["Profit"])
   plt.show()
   

** Results **
- The analysis provides insights into the composition of delivery costs and their impact on profit margins.
- Visualizations include a pie chart of cost distribution, a bar chart of overall financial summary, and a histogram of profit.

** Requirements **
- Python 3.x
- pandas
- matplotlib

This project offers a foundation for further analysis and optimization of food delivery costs to maximize profit margins.
