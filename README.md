# pandas-challenge-1
Module 4 Challenge
Wholesale Data Analysis Script
Welcome to the Client Data Analysis Script! This script helps you analyze client data from a CSV file to gather insights such as the most popular item categories, top clients, and financial summaries. If you're new to Python, don't worry—this guide will walk you through everything step by step.

Prerequisites
Before running the script, make sure you have the following:
1. Python: Ensure Python is installed on your system. You can download it from python.org.
2. Pandas Library: This script uses the pandas library. If you don't have it installed, you can install it using pip. Open your command prompt or terminal and run:
    1. pip install pandas
3. Getting Started
    Load the Data:
    The script starts by loading the client data from a CSV file named client_dataset.csv. Make sure this file is in the same directory as your script.
     1. import pandas as pd
     2. df = pd.read_csv('client_dataset.csv')
     3. df.head()
    Note: The head() function displays the first few rows of the dataset to give you an idea of what the data looks like.
4. Explore the Data:
   You can view the column names and basic statistics of the data to understand its structure.
    1. df["category"].value_counts()[:3]
5. Subcategory Analysis:
   For the category with the most entries (e.g., "consumables"), find the subcategory with the most entries. This narrows down the analysis to more specific items.
    1. df.loc[df['category'] == 'consumables','subcategory'].value_counts()[:1]
6. Top Clients:
   Identify the five clients with the most entries. These are your most active clients.
    1. df['client_id'].value_counts()[:5]

    # Store these client IDs in a list
    2. top5clients = list(df['client_id'].value_counts()[:5].index)

7. Client Orders:
   Calculate the total units ordered by the client with the most entries. This helps understand the order volume of your top client.
    1. df.loc[df["client_id"] == 33615]['qty'].sum()

8. Calculate Financials:
   Create new columns to calculate financial details like subtotal, shipping price, total price, line cost, and profit. These calculations give insights into the financial aspects of the orders.

   1. # Subtotal
      df['subtotal'] = df['unit_price'] * df['qty']

   2. # Shipping price
      df['order_weight'] = df['unit_weight'] * df['qty']
      def calculate_shipping_price(df):
        if df['order_weight'] > 50:
          return df['order_weight'] * 7 
        else:
          return df['order_weight'] * 10

    3. df['shipping_price'] = df.apply(calculate_shipping_price, axis=1)

    4. # Total price with sales tax
       df['total price'] = (df['subtotal'] + df['shipping_price']) * 1.0925

    5. # Cost of each line
       df['Cost of line'] = (df['unit_cost'] * df['qty']) + df['shipping_price']

    6. # Profit
       df['profit'] = df['total price'] - df['Cost of line']

9. Check Your Work:
   Verify your calculations by comparing totals for specific order IDs.
    1. print(df.loc[df['order_id'] == 2742071, "total price"].sum())
    2. print(df.loc[df['order_id'] == 2173913, "total price"].sum())
    3. print(df.loc[df['order_id'] == 6128929, "total price"].sum())

10. Top Clients' Spending:
    Calculate how much each of the top 5 clients spent. This helps in understanding the revenue contribution from your top clients.
    1. print(df.loc[df['client_id'] == 33615,'total price'].sum())
    2. print(df.loc[df['client_id'] == 66037,'total price'].sum())
    3. print(df.loc[df['client_id'] == 46820,'total price'].sum())
    4. print(df.loc[df['client_id'] == 38378,'total price'].sum())
    5. print(df.loc[df['client_id'] == 24741,'total price'].sum())

11. Summary DataFrame:
    Create a summary DataFrame for the top 5 clients, showing total units purchased, total shipping price, total revenue, and total profit.
    1. . data = []
       for client in top5clients:
        2. clientdf = df.loc[df['client_id'] == client]
        3. clientdict = {
             'client_id': client,
             'units_purchased': clientdf['qty'].sum(),
             'total_shipping_price': clientdf['shipping_price'].sum(),
             'total_revenue': clientdf['total price'].sum(),
             'total_profit': clientdf['profit'].sum()
           }
           data.append(clientdict)

        summary = pd.DataFrame(data)

12. Format and Sort Data:
    Format the financial data to show values in millions and sort by total profit. This makes the data more readable and highlights the most profitable clients.

    money_cols = ['total_shipping_price', 'total_revenue', 'total_profit']

    def to_millions(dollars):
       return dollars / 1_000_000

    summary[money_cols] = summary[money_cols].apply(to_millions)

    summary = summary.rename(columns={
       'total_shipping_price': 'total_shipping_price (millions)',
       'total_revenue': 'total_revenue (millions)',
       'total_profit': 'total_profit (millions)'
    })

    summary = summary.sort_values('total_profit (millions)', ascending=False)

13. Running the Script
    Simply run the script in your Python environment. Open your terminal or command prompt, navigate to the directory where your script is saved, and execute:
    1. python your_script_name.py


Make sure the client_dataset.csv file is in the same directory as your script. The script will output the results directly to the console.

Conclusion
Congratulations! You've now analyzed your client data and gained valuable insights. If you have any questions or need further assistance, feel free to reach out. Happy coding!