
# Heroes of Pymoli

## Observed Trends

### Trend 1
    The game is primarily played by men.
### Trend 2
    Most players are 20 - 24 years old.
### Trend 3
    The most popular item is the 'Betrayal, Whisper of Grieving Widows' 
        but the 'Retribution Axe' is the most popular.

## Importing Dependencies and Reading Json into a Data Frame


```python
# import dependencies
import pandas as pd
import numpy as np
```


```python
# open json file as data frame
json_file = "purchase_data.json"
purchase_df = pd.read_json(json_file, encoding="ISO-8859-1")
#print(type(player_data))
#purchase_df.head()
```


```python
# look for missing values (I know it isn't part of the hw but it is good practice to check)
#purchase_df.count()
```

## Player Count


```python
# total number of players

player_ndarray = purchase_df['SN'].unique()
#print(type(player_series))
player_count = (len(player_ndarray))
#print(player_count)

player_count_df = pd.DataFrame({"Total Players": [player_count]})
player_count_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Total Players</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>573</td>
    </tr>
  </tbody>
</table>
</div>



## Purchasing Analysis (Total)


```python
# unique item count
items_series = purchase_df['Item Name'].value_counts()
#print(type(items_series))
item_count = (len(items_series))
#print(item_count)
# mean purchase price
price_mean = purchase_df['Price'].mean()
#print(type(price_mean))
#print(str(price_mean))
# total number of purchases
purchase_count = (len(purchase_df.index))
#print(type(purchase_count))
#print(purchase_count)
# total revenue
revenue = purchase_df['Price'].sum()
#print(type(revenue))
#print(revenue)
purchase_summary_df = pd.DataFrame({"Number of Unique Items": [item_count],
                                    "Average Price": [price_mean],
                                    "Number of Purchases": [purchase_count],
                                    "Total Revenue": [revenue],
                                    })
purchase_summary_df["Average Price"] = purchase_summary_df["Average Price"].map("${:,.2f}".format)
purchase_summary_df["Total Revenue"] = purchase_summary_df["Total Revenue"].map("${:,.2f}".format)
purchase_summary_df

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Average Price</th>
      <th>Number of Purchases</th>
      <th>Number of Unique Items</th>
      <th>Total Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>$2.93</td>
      <td>780</td>
      <td>179</td>
      <td>$2,286.33</td>
    </tr>
  </tbody>
</table>
</div>



## Gender Demographics


```python
# df of players (not purchases) then group by gender
players_df = purchase_df.drop_duplicates(['SN'], keep= 'first')
grouped_gender= players_df.groupby(['Gender'])
# gender count
gender_count = grouped_gender['SN'].count()
#print(type(gender_count))
# gender percents
gender_pct = (gender_count/player_count)*100
#print(type(gender_pct))
# summary df and formatting
gender_summary_df = pd.DataFrame({"Player total": gender_count, 
                                  "Percentage of Players": gender_pct
                                 })
gender_summary_df = gender_summary_df[["Percentage of Players","Player total"]]
gender_summary_df["Percentage of Players"] = gender_summary_df["Percentage of Players"].map("{:,.2f}%".format)
gender_summary_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Percentage of Players</th>
      <th>Player total</th>
    </tr>
    <tr>
      <th>Gender</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Female</th>
      <td>17.45%</td>
      <td>100</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>81.15%</td>
      <td>465</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>1.40%</td>
      <td>8</td>
    </tr>
  </tbody>
</table>
</div>



## Purchasing Analysis (Gender)


```python
#group by gender
grouped_gender_sales = purchase_df.groupby(['Gender'])
# count of sales by gender
gender_sales_count = grouped_gender_sales['Price'].count()
# mean purchase by gender
gender_sales_ave = grouped_gender_sales['Price'].mean()
# total sales by gender
gender_sales_sum = grouped_gender_sales['Price'].sum()
# normalized totals by gender
gender_sales_nt = (gender_sales_sum/[gender for gender in gender_count])
# summary df and formatting
gender_purchase_summary_df = pd.DataFrame({
                                    "Purchase Count": gender_sales_count,
                                    "Average Purchase Price": round(gender_sales_ave, 2),
                                    "Total Purchase Value": gender_sales_sum,
                                    "Normalized Totals": round(gender_sales_nt,2)
                                    })
gender_purchase_summary_df = gender_purchase_summary_df[["Purchase Count", "Average Purchase Price","Total Purchase Value","Normalized Totals"]]
gender_purchase_summary_df["Average Purchase Price"] = gender_purchase_summary_df["Average Purchase Price"].map("${:,.2f}".format)
gender_purchase_summary_df["Total Purchase Value"] = gender_purchase_summary_df["Total Purchase Value"].map("${:,.2f}".format)                                                                                                          
gender_purchase_summary_df["Normalized Totals"] = gender_purchase_summary_df["Normalized Totals"].map("${:,.2f}".format) 
gender_purchase_summary_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
      <th>Normalized Totals</th>
    </tr>
    <tr>
      <th>Gender</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Female</th>
      <td>136</td>
      <td>$2.82</td>
      <td>$382.91</td>
      <td>$3.83</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>633</td>
      <td>$2.95</td>
      <td>$1,867.68</td>
      <td>$4.02</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>11</td>
      <td>$3.25</td>
      <td>$35.74</td>
      <td>$4.47</td>
    </tr>
  </tbody>
</table>
</div>



## Age Demographics


```python
# recode age from continuous to categorical
bins = [0, 10, 14, 19, 24, 29, 34, 39, 40]
group_names = ['< 10', '10 - 14', '15 - 19', '20 - 24', '25 - 29', '30 - 34', '35 - 39', '40+']
age_dem = pd.cut(players_df['Age'], bins, labels=group_names)
# age count
age_count = age_dem.value_counts()
# age percents
age_pct = (age_count/player_count)*100
# summary df and formatting
age_summary_df = pd.DataFrame({"Player total": age_count, 
                                  "Percentage of Players": age_pct                               
                                 })
age_summary_df = age_summary_df[["Percentage of Players","Player total"]]
age_summary_df = age_summary_df.reindex(group_names)
age_summary_df["Percentage of Players"] = age_summary_df["Percentage of Players"].map("{:,.2f}%".format)
age_summary_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Percentage of Players</th>
      <th>Player total</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt; 10</th>
      <td>3.84%</td>
      <td>22</td>
    </tr>
    <tr>
      <th>10 - 14</th>
      <td>3.49%</td>
      <td>20</td>
    </tr>
    <tr>
      <th>15 - 19</th>
      <td>17.45%</td>
      <td>100</td>
    </tr>
    <tr>
      <th>20 - 24</th>
      <td>45.20%</td>
      <td>259</td>
    </tr>
    <tr>
      <th>25 - 29</th>
      <td>15.18%</td>
      <td>87</td>
    </tr>
    <tr>
      <th>30 - 34</th>
      <td>8.20%</td>
      <td>47</td>
    </tr>
    <tr>
      <th>35 - 39</th>
      <td>4.71%</td>
      <td>27</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>1.40%</td>
      <td>8</td>
    </tr>
  </tbody>
</table>
</div>



## Purchasing Analysis (Age)


```python
#group by age category
age_sales = purchase_df.groupby(pd.cut(purchase_df['Age'], bins, labels=group_names))
# count of sales by age category
age_sales_count = age_sales['Price'].count()
# mean purchase by age category
age_sales_ave = age_sales['Price'].mean()
# total sales by age category
age_sales_sum = age_sales['Price'].sum()
# normalized totals by age category
age_sales_nt = (age_sales_sum/[bin for bin in age_count])
# summary df and formatting
age_purchase_summary_df = pd.DataFrame({
                                    "Purchase Count": age_sales_count,
                                    "Average Purchase Price": round(age_sales_ave, 2),
                                    "Total Purchase Value": age_sales_sum,
                                    "Normalized Totals": round(age_sales_nt,2)
                                    })
age_purchase_summary_df = age_purchase_summary_df[["Purchase Count", "Average Purchase Price","Total Purchase Value","Normalized Totals"]]
age_purchase_summary_df["Average Purchase Price"] = age_purchase_summary_df["Average Purchase Price"].map("${:,.2f}".format)
age_purchase_summary_df["Total Purchase Value"] = age_purchase_summary_df["Total Purchase Value"].map("${:,.2f}".format)                                                                                                          
age_purchase_summary_df["Normalized Totals"] = age_purchase_summary_df["Normalized Totals"].map("${:,.2f}".format)      
age_purchase_summary_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
      <th>Normalized Totals</th>
    </tr>
    <tr>
      <th>Age</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt; 10</th>
      <td>32</td>
      <td>$3.02</td>
      <td>$96.62</td>
      <td>$0.37</td>
    </tr>
    <tr>
      <th>10 - 14</th>
      <td>31</td>
      <td>$2.70</td>
      <td>$83.79</td>
      <td>$0.84</td>
    </tr>
    <tr>
      <th>15 - 19</th>
      <td>133</td>
      <td>$2.91</td>
      <td>$386.42</td>
      <td>$4.44</td>
    </tr>
    <tr>
      <th>20 - 24</th>
      <td>336</td>
      <td>$2.91</td>
      <td>$978.77</td>
      <td>$20.82</td>
    </tr>
    <tr>
      <th>25 - 29</th>
      <td>125</td>
      <td>$2.96</td>
      <td>$370.33</td>
      <td>$13.72</td>
    </tr>
    <tr>
      <th>30 - 34</th>
      <td>64</td>
      <td>$3.08</td>
      <td>$197.25</td>
      <td>$8.97</td>
    </tr>
    <tr>
      <th>35 - 39</th>
      <td>42</td>
      <td>$2.84</td>
      <td>$119.40</td>
      <td>$5.97</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>14</td>
      <td>$3.22</td>
      <td>$45.11</td>
      <td>$5.64</td>
    </tr>
  </tbody>
</table>
</div>



## Top Spenders


```python
# group by SN
top_spenders = purchase_df.groupby(['SN'])
# purchase count per SN
players_sales_count = top_spenders['Price'].count()
# average purchase price per SN
players_sales_mean = top_spenders['Price'].mean()
# total sales per SN
players_sales_sum = top_spenders['Price'].sum()
# summary df, sorting, and formatting
top_spenders_summary_df = pd.DataFrame({
                                    "Purchase Count": players_sales_count,
                                    "Average Purchase Price": round(players_sales_mean, 2),
                                    "Total Purchase Value": players_sales_sum
                                    })
top_spenders_summary_df = top_spenders_summary_df[["Purchase Count", "Average Purchase Price","Total Purchase Value"]]
top_spenders_summary_df = top_spenders_summary_df.sort_values(['Total Purchase Value'], ascending=[False])
top_spenders_summary_df["Total Purchase Value"] = top_spenders_summary_df["Total Purchase Value"].map("${:,.2f}".format)
top_spenders_summary_df["Average Purchase Price"] = top_spenders_summary_df["Average Purchase Price"].map("${:,.2f}".format)
top_spenders_summary_df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>SN</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Undirrala66</th>
      <td>5</td>
      <td>$3.41</td>
      <td>$17.06</td>
    </tr>
    <tr>
      <th>Saedue76</th>
      <td>4</td>
      <td>$3.39</td>
      <td>$13.56</td>
    </tr>
    <tr>
      <th>Mindimnya67</th>
      <td>4</td>
      <td>$3.18</td>
      <td>$12.74</td>
    </tr>
    <tr>
      <th>Haellysu29</th>
      <td>3</td>
      <td>$4.24</td>
      <td>$12.73</td>
    </tr>
    <tr>
      <th>Eoda93</th>
      <td>3</td>
      <td>$3.86</td>
      <td>$11.58</td>
    </tr>
  </tbody>
</table>
</div>



## Most Popular Items


```python
#group by item id number
top_items = purchase_df.groupby(['Item ID'])
# item name
top_name = top_items['Item Name'].unique().str[0]
# price per item
top_price = top_items['Price'].unique().str[0]
#purchase count per item
top_count = top_items['Price'].count()
# total sales per item
top_sum = top_items['Price'].sum()
#print(type(top_price))
# summary df, sorting (including next table as the formatting forces this order), and formatting
top_purchase_summary_df = pd.DataFrame({
                                        "Purchase Count": top_count,
                                        "Total Purchase Value": top_sum,
                                        "Item Name": top_name,
                                        "Item Price": top_price
                                        })
top_purchase_summary_df = top_purchase_summary_df[["Item Name","Purchase Count", "Item Price","Total Purchase Value"]]
top_purchase_summary_df = top_purchase_summary_df.sort_values(["Purchase Count"], ascending=[False])
top_profit_summary_df = top_purchase_summary_df.sort_values(["Total Purchase Value"], ascending=[False])
top_purchase_summary_df["Item Price"] = top_purchase_summary_df["Item Price"].map("${:,.2f}".format)
top_purchase_summary_df["Total Purchase Value"] = top_purchase_summary_df["Total Purchase Value"].map("${:,.2f}".format)
top_purchase_summary_df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Item Name</th>
      <th>Purchase Count</th>
      <th>Item Price</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Item ID</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>39</th>
      <td>Betrayal, Whisper of Grieving Widows</td>
      <td>11</td>
      <td>$2.35</td>
      <td>$25.85</td>
    </tr>
    <tr>
      <th>84</th>
      <td>Arcane Gem</td>
      <td>11</td>
      <td>$2.23</td>
      <td>$24.53</td>
    </tr>
    <tr>
      <th>31</th>
      <td>Trickster</td>
      <td>9</td>
      <td>$2.07</td>
      <td>$18.63</td>
    </tr>
    <tr>
      <th>175</th>
      <td>Woeful Adamantite Claymore</td>
      <td>9</td>
      <td>$1.24</td>
      <td>$11.16</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Serenity</td>
      <td>9</td>
      <td>$1.49</td>
      <td>$13.41</td>
    </tr>
  </tbody>
</table>
</div>



## Most Profitable Items


```python
# summary df and formatting
top_profit_summary_df["Item Price"] = top_profit_summary_df["Item Price"].map("${:,.2f}".format)
top_profit_summary_df["Total Purchase Value"] = top_profit_summary_df["Total Purchase Value"].map("${:,.2f}".format)
top_profit_summary_df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Item Name</th>
      <th>Purchase Count</th>
      <th>Item Price</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Item ID</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>34</th>
      <td>Retribution Axe</td>
      <td>9</td>
      <td>$4.14</td>
      <td>$37.26</td>
    </tr>
    <tr>
      <th>115</th>
      <td>Spectral Diamond Doomblade</td>
      <td>7</td>
      <td>$4.25</td>
      <td>$29.75</td>
    </tr>
    <tr>
      <th>32</th>
      <td>Orenmir</td>
      <td>6</td>
      <td>$4.95</td>
      <td>$29.70</td>
    </tr>
    <tr>
      <th>103</th>
      <td>Singed Scalpel</td>
      <td>6</td>
      <td>$4.87</td>
      <td>$29.22</td>
    </tr>
    <tr>
      <th>107</th>
      <td>Splitter, Foe Of Subtlety</td>
      <td>8</td>
      <td>$3.61</td>
      <td>$28.88</td>
    </tr>
  </tbody>
</table>
</div>


