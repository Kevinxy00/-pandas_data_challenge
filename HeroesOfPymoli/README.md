

```python
import pandas as pd

#---Change the file name here!---
filePath1 = r'purchase_data2.json'
#filePath2 = r'purchase_data2.json'

Purchases_df = pd.read_json(filePath1)
#Purchases2_df = pd.read_json(filePath2)

Purchases_df.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Age</th>
      <th>Gender</th>
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Price</th>
      <th>SN</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>20</td>
      <td>Male</td>
      <td>93</td>
      <td>Apocalyptic Battlescythe</td>
      <td>4.49</td>
      <td>Iloni35</td>
    </tr>
    <tr>
      <th>1</th>
      <td>21</td>
      <td>Male</td>
      <td>12</td>
      <td>Dawne</td>
      <td>3.36</td>
      <td>Aidaira26</td>
    </tr>
    <tr>
      <th>2</th>
      <td>17</td>
      <td>Male</td>
      <td>5</td>
      <td>Putrid Fan</td>
      <td>2.63</td>
      <td>Irim47</td>
    </tr>
    <tr>
      <th>3</th>
      <td>17</td>
      <td>Male</td>
      <td>123</td>
      <td>Twilight's Carver</td>
      <td>2.55</td>
      <td>Irith83</td>
    </tr>
    <tr>
      <th>4</th>
      <td>22</td>
      <td>Male</td>
      <td>154</td>
      <td>Feral Katana</td>
      <td>4.11</td>
      <td>Philodil43</td>
    </tr>
  </tbody>
</table>
</div>




```python
# **Player Count**
# Get no. of unique player names. Be careful of players who purchased multiple times!
player_cnt = Purchases_df['SN'].nunique()

#Create table of player count, starting with turning player_cnt into series to get rid of aesthetic errors 
player_cnt_S = pd.Series(player_cnt)
player_cnt_df = pd.DataFrame(player_cnt_S)
player_cnt_df.columns = ['Total Players']

player_cnt_df
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
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
      <td>74</td>
    </tr>
  </tbody>
</table>
</div>




```python
# **Purchasing Analysis (Total)**
'''* Number of Unique Items * Average Purchase Price * Total Number of Purchases * Total Revenue'''

uniq_item = Purchases_df['Item Name'].nunique()
avgPurPrice = Purchases_df['Price'].mean()
totlNumPurch = Purchases_df['Price'].count()
totlRev = avgPurPrice * totlNumPurch

PurchAnalys_df = pd.DataFrame({'Number of Unique Items':uniq_item, 'Average Price':avgPurPrice,\
                               'Number of Purchases':totlNumPurch, 'Total Revenue':totlRev}, index = [0])
PurchAnalys_df.reindex(columns = ['Number of Unique Items', 'Average Price',\
                                  'Number of Purchases', 'Total Revenue'])

```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Number of Unique Items</th>
      <th>Average Price</th>
      <th>Number of Purchases</th>
      <th>Total Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>63</td>
      <td>2.924359</td>
      <td>78</td>
      <td>228.1</td>
    </tr>
  </tbody>
</table>
</div>




```python
# **Gender Demographics**
'''* Percentage and Count of Male Players * Percentage and Count of Female Players 
* Percentage and Count of Other / Non-Disclosed'''

#initializing some useful variables
IsMale = Purchases_df['Gender']=='Male'
IsFemale = Purchases_df['Gender']=='Female'
IsNoGend = (Purchases_df['Gender']!='Male') & (Purchases_df['Gender']!='Female')
indexGen = ['Male', 'Female', 'Other/Non-Disclosed']

#getting count of all player genders, careful of repeat purchases
MaleCt =  Purchases_df[IsMale]['SN'].nunique()
FemCt = Purchases_df[IsFemale ]['SN'].nunique()
NAGendCt = Purchases_df[IsNoGend]['SN'].nunique()
GendCt = pd.Series([MaleCt, FemCt, NAGendCt], index=indexGen)

#getting % of all player genders
MalePerc = MaleCt/player_cnt #Purch_cnt ("total purchase count") from previous section **player count**
FemPerc = FemCt/player_cnt
NAPerc = NAGendCt/player_cnt
GendPerc = pd.Series([MalePerc, FemPerc, NAPerc], index=indexGen)

Gen_df = pd.DataFrame({'Percentage of Players':GendPerc, 'Total Count':GendCt})

Gen_df
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Percentage of Players</th>
      <th>Total Count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Male</th>
      <td>0.810811</td>
      <td>60</td>
    </tr>
    <tr>
      <th>Female</th>
      <td>0.175676</td>
      <td>13</td>
    </tr>
    <tr>
      <th>Other/Non-Disclosed</th>
      <td>0.013514</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>




```python
# **Purchasing Analysis (Gender)** 
'''Broken by gender:* Purchase Count * Average Purchase Price * Total Purchase Value * Normalized Totals'''

# I count multiple purchases by the same person 
MaleCt2 = Purchases_df[IsMale].count()
MaleCt2 = MaleCt2['SN']
FemCt2 = Purchases_df[IsFemale].count()
FemCt2 = FemCt2['SN']
NAGendCt2 = Purchases_df[IsNoGend].count()
NAGendCt2 = NAGendCt2['SN']
GendCt_s = pd.Series([MaleCt2, FemCt2, NAGendCt2], index=indexGen)

avgPurchPr_M = Purchases_df[IsMale]['Price'].mean()
avgPurchPr_F = Purchases_df[IsFemale]['Price'].mean()
avgPurchPr_N = Purchases_df[IsNoGend]['Price'].mean()
GendPurchAvg_s = pd.Series([avgPurchPr_M, avgPurchPr_F, avgPurchPr_N], index = indexGen)

totlPurchV_M =  Purchases_df[IsMale]['Price'].sum()
totlPurchV_F = Purchases_df[IsFemale]['Price'].sum()
totlPurchV_N = Purchases_df[IsNoGend]['Price'].sum()
GendPurchTot_s = pd.Series([totlPurchV_M, totlPurchV_F, totlPurchV_N], index = indexGen)

#not sure what is meant by Normalized Totals. I'm guessing it's the average of the totals per person of each gender
#The count (...Ct) variables taken from **Gender Demographics** section. They only include unique SNs 
normTotl_M = totlPurchV_M / MaleCt
normTotl_F = totlPurchV_F / FemCt
normTotl_N = totlPurchV_N / NAGendCt
GendNormTot_s = pd.Series([normTotl_M, normTotl_F, normTotl_N], index = indexGen)

# Create new dataframe with all the data series put together
GendPurch_df = pd.DataFrame({'Purchase Count':GendCt_s, 'Average Purchase Price':GendPurchAvg_s,\
                            'Total Purchase Value':GendPurchTot_s, 'Normalized Totals':GendNormTot_s})
GendPurch_df.reindex(columns=['Purchase Count', 'Average Purchase Price', 'Total Purchase Value', 'Normalized Totals'])
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
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
  </thead>
  <tbody>
    <tr>
      <th>Male</th>
      <td>64</td>
      <td>2.884375</td>
      <td>184.60</td>
      <td>3.076667</td>
    </tr>
    <tr>
      <th>Female</th>
      <td>13</td>
      <td>3.183077</td>
      <td>41.38</td>
      <td>3.183077</td>
    </tr>
    <tr>
      <th>Other/Non-Disclosed</th>
      <td>1</td>
      <td>2.120000</td>
      <td>2.12</td>
      <td>2.120000</td>
    </tr>
  </tbody>
</table>
</div>




```python
#**Age Demographics**
''' The below each broken into bins of 4 years (i.e. &lt;10, 10-14, 15-19, etc.) 
* Purchase Count * Average Purchase Price * Total Purchase Value * Normalized Totals'''

#Purchases_df['Age']: max==40, min==7
#create bins and append 'Age Range' to Purchases_df
bins=[0, 10, 15, 20, 25, 30, 35, 40, 150]
labels=['<10', '10-14', '15-19', '20-24', '25-29', '30-34', '35-39', '40+']
Purchases_df['Age Range'] = pd.cut(Purchases_df['Age'], bins=bins, right=False, labels=labels)
PurchAges = Purchases_df.groupby('Age Range')

#Purchase Count
PurchAgeCt = PurchAges['SN'].count()
#Average Purchase Price
avgPurchP = PurchAges['Price'].mean()
#Total Purchase Value
totlPurchV = PurchAges['Price'].sum()
#Normalized Totals. I'm guessing this to be (total/# of players) for each age group
normPurchV = totlPurchV / PurchAges['SN'].nunique()

#concatenate ALL the columns!!!
AgePurch_df = pd.concat({'Purchase Count':PurchAgeCt, 'Average Purchase Price':avgPurchP,\
                            'Total Purchase Value':totlPurchV, 'Normalized Totals':normPurchV}, axis=1)
#fix column order
AgePurch_df.reindex(columns = ['Purchase Count', 'Average Purchase Price', 'Total Purchase Value', 'Normalized Totals'])
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
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
      <th>Age Range</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>5</td>
      <td>2.764000</td>
      <td>13.82</td>
      <td>2.764000</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>3</td>
      <td>2.986667</td>
      <td>8.96</td>
      <td>2.986667</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>11</td>
      <td>2.764545</td>
      <td>30.41</td>
      <td>2.764545</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>36</td>
      <td>3.024722</td>
      <td>108.89</td>
      <td>3.202647</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>9</td>
      <td>2.901111</td>
      <td>26.11</td>
      <td>3.263750</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>7</td>
      <td>1.984286</td>
      <td>13.89</td>
      <td>2.315000</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>6</td>
      <td>3.561667</td>
      <td>21.37</td>
      <td>3.561667</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>1</td>
      <td>4.650000</td>
      <td>4.65</td>
      <td>4.650000</td>
    </tr>
  </tbody>
</table>
</div>




```python
# **Top Spenders**
''' Identify the the top 5 spenders in the game by total purchase value, then list (in a table):
* SN, Purchase Count, Average Purchase Price, Total Purchase Value '''
#group by each player
TopSpends = Purchases_df.groupby('SN')

#getting Purchase Count, Average Purchase Price, Total Purchase Value 
PurchCt_TS = TopSpends['Price'].count()
avgPr_TS = TopSpends['Price'].mean()
totlPurchV_TS = TopSpends['Price'].sum()

#concat into a df, sort by largest total purchases by each player, and fix column order
TopS_df = pd.concat({'Purchase Count': PurchCt_TS, 'Average Purchase Price': avgPr_TS,\
                     'Total Purchase Value':totlPurchV_TS}, axis=1)
TopS_df = TopS_df.sort_values(by='Total Purchase Value', ascending=False)
TopS_df = TopS_df.reindex(columns=['Purchase Count', 'Average Purchase Price', 'Total Purchase Value'])
TopS_df.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
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
      <th>Sundaky74</th>
      <td>2</td>
      <td>3.705</td>
      <td>7.41</td>
    </tr>
    <tr>
      <th>Aidaira26</th>
      <td>2</td>
      <td>2.565</td>
      <td>5.13</td>
    </tr>
    <tr>
      <th>Eusty71</th>
      <td>1</td>
      <td>4.810</td>
      <td>4.81</td>
    </tr>
    <tr>
      <th>Chanirra64</th>
      <td>1</td>
      <td>4.780</td>
      <td>4.78</td>
    </tr>
    <tr>
      <th>Alarap40</th>
      <td>1</td>
      <td>4.710</td>
      <td>4.71</td>
    </tr>
  </tbody>
</table>
</div>




```python
# **Most Popular Items**
'''Identify the 5 most popular items by purchase count, then list (in a table):
* Item ID, Item Name, Purchase Count, Item Price, Total Purchase Value'''
#initial group by
Item_GBy = Purchases_df.groupby('Item ID')

#Getting Item Name, Purchase Count, Item Price, Total Purchase Value
#ItmName and ItmPr only works when a function is called to them 
ItmName = Item_GBy['Item Name'].unique()
PurchCt = Item_GBy['Price'].count()
ItmPr = Item_GBy['Price'].unique()
totlItemV = Item_GBy['Price'].sum()

#Form vars into new dataframe, sort first by Purchase count, then by total purchase value, then format & view
PopItm_df = pd.DataFrame({'Item Name':ItmName, 'Purchase Count': PurchCt,\
                      'Item Price':ItmPr, 'Total Purchase Value': totlItemV})
PopItm_df = PopItm_df.sort_values(['Purchase Count', 'Total Purchase Value'], ascending=False)
PopItm_df = PopItm_df.reindex(columns=['Item Name', 'Purchase Count', 'Item Price', 'Total Purchase Value'])
PopItm_df.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
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
      <th>94</th>
      <td>[Mourning Blade]</td>
      <td>3</td>
      <td>[3.64]</td>
      <td>10.92</td>
    </tr>
    <tr>
      <th>117</th>
      <td>[Heartstriker, Legacy of the Light]</td>
      <td>2</td>
      <td>[4.71]</td>
      <td>9.42</td>
    </tr>
    <tr>
      <th>93</th>
      <td>[Apocalyptic Battlescythe]</td>
      <td>2</td>
      <td>[4.49]</td>
      <td>8.98</td>
    </tr>
    <tr>
      <th>90</th>
      <td>[Betrayer]</td>
      <td>2</td>
      <td>[4.12]</td>
      <td>8.24</td>
    </tr>
    <tr>
      <th>154</th>
      <td>[Feral Katana]</td>
      <td>2</td>
      <td>[4.11]</td>
      <td>8.22</td>
    </tr>
  </tbody>
</table>
</div>




```python
# **Most Profitable Items**
''' Identify the 5 most profitable items by total purchase value, then list (in a table):
* Item ID, Item Name, Purchase Count, Item Price, Total Purchase Value '''

#Using variables & DF from previous section **Most Popular Items**
ProfItm_df = PopItm_df.sort_values('Total Purchase Value', ascending=False)
ProfItm_df.head()

```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
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
      <th>94</th>
      <td>[Mourning Blade]</td>
      <td>3</td>
      <td>[3.64]</td>
      <td>10.92</td>
    </tr>
    <tr>
      <th>117</th>
      <td>[Heartstriker, Legacy of the Light]</td>
      <td>2</td>
      <td>[4.71]</td>
      <td>9.42</td>
    </tr>
    <tr>
      <th>93</th>
      <td>[Apocalyptic Battlescythe]</td>
      <td>2</td>
      <td>[4.49]</td>
      <td>8.98</td>
    </tr>
    <tr>
      <th>90</th>
      <td>[Betrayer]</td>
      <td>2</td>
      <td>[4.12]</td>
      <td>8.24</td>
    </tr>
    <tr>
      <th>154</th>
      <td>[Feral Katana]</td>
      <td>2</td>
      <td>[4.11]</td>
      <td>8.22</td>
    </tr>
  </tbody>
</table>
</div>


