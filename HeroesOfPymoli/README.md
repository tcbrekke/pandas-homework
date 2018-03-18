

```python
#Observed Trends:

#Trend 1: The user base for this game is overwhelmingly male - it may be prudent to investigate why there aren't more non-male players
#Trend 2: The user base is highly concentrated, with the majority of players being in their 20s - there may be room to grow the userbase on either side of this group, with teens between 15 and 19 appearing to be a strong area for growth
#Trend 3: When looking to generate additional revenue, it may be best to emphasize quality over quantity - the leading revenue generators sold fewer units than the most-sold items, but generated higher revenue due to a higher cost per unit
```


```python
import os
import pandas as pd
import warnings
warnings.filterwarnings('ignore')
```


```python
raw_data_path = "purchase_data.json"

raw_data = pd.read_json(raw_data_path)
```


```python
raw_data_df = pd.DataFrame(raw_data)
```


```python
raw_data_df.head()
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
      <td>38</td>
      <td>Male</td>
      <td>165</td>
      <td>Bone Crushing Silver Skewer</td>
      <td>3.37</td>
      <td>Aelalis34</td>
    </tr>
    <tr>
      <th>1</th>
      <td>21</td>
      <td>Male</td>
      <td>119</td>
      <td>Stormbringer, Dark Blade of Ending Misery</td>
      <td>2.32</td>
      <td>Eolo46</td>
    </tr>
    <tr>
      <th>2</th>
      <td>34</td>
      <td>Male</td>
      <td>174</td>
      <td>Primitive Blade</td>
      <td>2.46</td>
      <td>Assastnya25</td>
    </tr>
    <tr>
      <th>3</th>
      <td>21</td>
      <td>Male</td>
      <td>92</td>
      <td>Final Critic</td>
      <td>1.36</td>
      <td>Pheusrical25</td>
    </tr>
    <tr>
      <th>4</th>
      <td>23</td>
      <td>Male</td>
      <td>63</td>
      <td>Stormfury Mace</td>
      <td>1.27</td>
      <td>Aela59</td>
    </tr>
  </tbody>
</table>
</div>




```python
deduped_df = raw_data_df.drop_duplicates(["SN"])
no_of_players_df = pd.DataFrame({"Total Players":[len(deduped_df)]})
no_of_players_df

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




```python
total_players = 573
unique_items = raw_data_df["Item ID"].unique()
no_of_items = len(unique_items)
average_price = raw_data_df["Price"].mean()
total_purchases = raw_data_df["Price"].count()
total_revenue = raw_data_df["Price"].sum()
topline_items_df = pd.DataFrame([{"Number of Unique Items":no_of_items,
                                  "Average Item Price":average_price,
                                  "Total Purchases":total_purchases,
                                  "Total Revenue":total_revenue,}])

topline_items_df.style.format({"Average Item Price":"${:,.2f}", "Total Revenue":"${:,.2f}"})
```




<style  type="text/css" >
</style>  
<table id="T_f3001928_2afc_11e8_8618_9a00ccc28801" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Average Item Price</th> 
        <th class="col_heading level0 col1" >Number of Unique Items</th> 
        <th class="col_heading level0 col2" >Total Purchases</th> 
        <th class="col_heading level0 col3" >Total Revenue</th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_f3001928_2afc_11e8_8618_9a00ccc28801level0_row0" class="row_heading level0 row0" >0</th> 
        <td id="T_f3001928_2afc_11e8_8618_9a00ccc28801row0_col0" class="data row0 col0" >$2.93</td> 
        <td id="T_f3001928_2afc_11e8_8618_9a00ccc28801row0_col1" class="data row0 col1" >183</td> 
        <td id="T_f3001928_2afc_11e8_8618_9a00ccc28801row0_col2" class="data row0 col2" >780</td> 
        <td id="T_f3001928_2afc_11e8_8618_9a00ccc28801row0_col3" class="data row0 col3" >$2,286.33</td> 
    </tr></tbody> 
</table> 




```python
#Create dataframes broken down by gender
male_data_df = raw_data_df.loc[raw_data_df["Gender"]=="Male"]
female_data_df = raw_data_df.loc[raw_data_df["Gender"]=="Female"]
nd_data_df = raw_data_df.loc[(raw_data_df["Gender"]!="Male") & (raw_data_df["Gender"]!="Female")]

#Find number of unique players by gender
unique_male_players = male_data_df["SN"].unique()
total_male_players = len(unique_male_players)
unique_female_players = female_data_df["SN"].unique()
total_female_players = len(unique_female_players)
unique_nd_players = nd_data_df["SN"].unique()
total_nd_players = len(unique_nd_players)

gender_breakdown_df = pd.DataFrame({"Gender":["Male","Female","Other / Non-Disclosed"],
                                   "Total Players":[total_male_players,total_female_players,total_nd_players],
                                   "Percentage of Players":[(total_male_players/total_players)*100,(total_female_players/total_players)*100,(total_nd_players/total_players)*100]})

gender_breakdown_df.style.format({"Percentage of Players":"{:,.2f}%"})
```




<style  type="text/css" >
</style>  
<table id="T_f312f610_2afc_11e8_8fd6_9a00ccc28801" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Gender</th> 
        <th class="col_heading level0 col1" >Percentage of Players</th> 
        <th class="col_heading level0 col2" >Total Players</th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_f312f610_2afc_11e8_8fd6_9a00ccc28801level0_row0" class="row_heading level0 row0" >0</th> 
        <td id="T_f312f610_2afc_11e8_8fd6_9a00ccc28801row0_col0" class="data row0 col0" >Male</td> 
        <td id="T_f312f610_2afc_11e8_8fd6_9a00ccc28801row0_col1" class="data row0 col1" >81.15%</td> 
        <td id="T_f312f610_2afc_11e8_8fd6_9a00ccc28801row0_col2" class="data row0 col2" >465</td> 
    </tr>    <tr> 
        <th id="T_f312f610_2afc_11e8_8fd6_9a00ccc28801level0_row1" class="row_heading level0 row1" >1</th> 
        <td id="T_f312f610_2afc_11e8_8fd6_9a00ccc28801row1_col0" class="data row1 col0" >Female</td> 
        <td id="T_f312f610_2afc_11e8_8fd6_9a00ccc28801row1_col1" class="data row1 col1" >17.45%</td> 
        <td id="T_f312f610_2afc_11e8_8fd6_9a00ccc28801row1_col2" class="data row1 col2" >100</td> 
    </tr>    <tr> 
        <th id="T_f312f610_2afc_11e8_8fd6_9a00ccc28801level0_row2" class="row_heading level0 row2" >2</th> 
        <td id="T_f312f610_2afc_11e8_8fd6_9a00ccc28801row2_col0" class="data row2 col0" >Other / Non-Disclosed</td> 
        <td id="T_f312f610_2afc_11e8_8fd6_9a00ccc28801row2_col1" class="data row2 col1" >1.40%</td> 
        <td id="T_f312f610_2afc_11e8_8fd6_9a00ccc28801row2_col2" class="data row2 col2" >8</td> 
    </tr></tbody> 
</table> 




```python
#Find averages per gender
m_avg_price = male_data_df["Price"].mean()
f_avg_price = female_data_df["Price"].mean()
nd_avg_price = nd_data_df["Price"].mean()

m_total_revenue = male_data_df["Price"].sum()
f_total_revenue = female_data_df["Price"].sum()
nd_total_revenue = nd_data_df["Price"].sum()
m_total_norm = m_total_revenue / total_male_players
f_total_norm = f_total_revenue / total_female_players
nd_total_norm = nd_total_revenue / total_nd_players

purchases_gender_df = pd.DataFrame({"Gender":["Male","Female","Other / Non-Disclosed"],
                                    "Total Players":[total_male_players,total_female_players,total_nd_players],
                                    "Average Purchase Price":[m_avg_price,f_avg_price,nd_avg_price],
                                    "Total Purchase Value":[m_total_revenue,f_total_revenue,nd_total_revenue],
                                    "Normalized Total":[m_total_norm,f_total_norm,nd_total_norm]})

purchases_gender_df.style.format({"Average Purchase Price":"${:,.2f}","Total Purchase Value":"${:,.2f}","Normalized Total":"${:,.2f}"})
```




<style  type="text/css" >
</style>  
<table id="T_f334341a_2afc_11e8_9403_9a00ccc28801" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Average Purchase Price</th> 
        <th class="col_heading level0 col1" >Gender</th> 
        <th class="col_heading level0 col2" >Normalized Total</th> 
        <th class="col_heading level0 col3" >Total Players</th> 
        <th class="col_heading level0 col4" >Total Purchase Value</th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_f334341a_2afc_11e8_9403_9a00ccc28801level0_row0" class="row_heading level0 row0" >0</th> 
        <td id="T_f334341a_2afc_11e8_9403_9a00ccc28801row0_col0" class="data row0 col0" >$2.95</td> 
        <td id="T_f334341a_2afc_11e8_9403_9a00ccc28801row0_col1" class="data row0 col1" >Male</td> 
        <td id="T_f334341a_2afc_11e8_9403_9a00ccc28801row0_col2" class="data row0 col2" >$4.02</td> 
        <td id="T_f334341a_2afc_11e8_9403_9a00ccc28801row0_col3" class="data row0 col3" >465</td> 
        <td id="T_f334341a_2afc_11e8_9403_9a00ccc28801row0_col4" class="data row0 col4" >$1,867.68</td> 
    </tr>    <tr> 
        <th id="T_f334341a_2afc_11e8_9403_9a00ccc28801level0_row1" class="row_heading level0 row1" >1</th> 
        <td id="T_f334341a_2afc_11e8_9403_9a00ccc28801row1_col0" class="data row1 col0" >$2.82</td> 
        <td id="T_f334341a_2afc_11e8_9403_9a00ccc28801row1_col1" class="data row1 col1" >Female</td> 
        <td id="T_f334341a_2afc_11e8_9403_9a00ccc28801row1_col2" class="data row1 col2" >$3.83</td> 
        <td id="T_f334341a_2afc_11e8_9403_9a00ccc28801row1_col3" class="data row1 col3" >100</td> 
        <td id="T_f334341a_2afc_11e8_9403_9a00ccc28801row1_col4" class="data row1 col4" >$382.91</td> 
    </tr>    <tr> 
        <th id="T_f334341a_2afc_11e8_9403_9a00ccc28801level0_row2" class="row_heading level0 row2" >2</th> 
        <td id="T_f334341a_2afc_11e8_9403_9a00ccc28801row2_col0" class="data row2 col0" >$3.25</td> 
        <td id="T_f334341a_2afc_11e8_9403_9a00ccc28801row2_col1" class="data row2 col1" >Other / Non-Disclosed</td> 
        <td id="T_f334341a_2afc_11e8_9403_9a00ccc28801row2_col2" class="data row2 col2" >$4.47</td> 
        <td id="T_f334341a_2afc_11e8_9403_9a00ccc28801row2_col3" class="data row2 col3" >8</td> 
        <td id="T_f334341a_2afc_11e8_9403_9a00ccc28801row2_col4" class="data row2 col4" >$35.74</td> 
    </tr></tbody> 
</table> 




```python
age_bins = [0,10,15,20,25,30,35,40,45,50,1000]
age_labels=["<10","10-14","15-19","20-24","25-29","30-34","35-39","40-44","45-49",">50"]

deduped_df["Age Range"] = pd.cut(deduped_df["Age"], age_bins, labels = age_labels, right = False)
raw_data_df["Age Range"] = pd.cut(raw_data_df["Age"], age_bins, labels = age_labels, right = False)

group_by_ages = raw_data_df.groupby("Age Range")
unique_age_range_members = deduped_df.groupby("Age Range").size()
age_avg_price = group_by_ages["Price"].mean()
age_total_revenue = group_by_ages["Price"].sum()
age_total_purchases = group_by_ages.size()
age_revenue_norm = age_total_revenue / unique_age_range_members

population_breakdown_by_age = pd.DataFrame({"Percentage of Players": (unique_age_range_members / total_players) * 100,
                                           "Total Individuals": unique_age_range_members})
population_breakdown_by_age.style.format({"Percentage of Players":"{:,.2f}%"})

```




<style  type="text/css" >
</style>  
<table id="T_f38f10e2_2afc_11e8_919e_9a00ccc28801" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Percentage of Players</th> 
        <th class="col_heading level0 col1" >Total Individuals</th> 
    </tr>    <tr> 
        <th class="index_name level0" >Age Range</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_f38f10e2_2afc_11e8_919e_9a00ccc28801level0_row0" class="row_heading level0 row0" ><10</th> 
        <td id="T_f38f10e2_2afc_11e8_919e_9a00ccc28801row0_col0" class="data row0 col0" >3.32%</td> 
        <td id="T_f38f10e2_2afc_11e8_919e_9a00ccc28801row0_col1" class="data row0 col1" >19</td> 
    </tr>    <tr> 
        <th id="T_f38f10e2_2afc_11e8_919e_9a00ccc28801level0_row1" class="row_heading level0 row1" >10-14</th> 
        <td id="T_f38f10e2_2afc_11e8_919e_9a00ccc28801row1_col0" class="data row1 col0" >4.01%</td> 
        <td id="T_f38f10e2_2afc_11e8_919e_9a00ccc28801row1_col1" class="data row1 col1" >23</td> 
    </tr>    <tr> 
        <th id="T_f38f10e2_2afc_11e8_919e_9a00ccc28801level0_row2" class="row_heading level0 row2" >15-19</th> 
        <td id="T_f38f10e2_2afc_11e8_919e_9a00ccc28801row2_col0" class="data row2 col0" >17.45%</td> 
        <td id="T_f38f10e2_2afc_11e8_919e_9a00ccc28801row2_col1" class="data row2 col1" >100</td> 
    </tr>    <tr> 
        <th id="T_f38f10e2_2afc_11e8_919e_9a00ccc28801level0_row3" class="row_heading level0 row3" >20-24</th> 
        <td id="T_f38f10e2_2afc_11e8_919e_9a00ccc28801row3_col0" class="data row3 col0" >45.20%</td> 
        <td id="T_f38f10e2_2afc_11e8_919e_9a00ccc28801row3_col1" class="data row3 col1" >259</td> 
    </tr>    <tr> 
        <th id="T_f38f10e2_2afc_11e8_919e_9a00ccc28801level0_row4" class="row_heading level0 row4" >25-29</th> 
        <td id="T_f38f10e2_2afc_11e8_919e_9a00ccc28801row4_col0" class="data row4 col0" >15.18%</td> 
        <td id="T_f38f10e2_2afc_11e8_919e_9a00ccc28801row4_col1" class="data row4 col1" >87</td> 
    </tr>    <tr> 
        <th id="T_f38f10e2_2afc_11e8_919e_9a00ccc28801level0_row5" class="row_heading level0 row5" >30-34</th> 
        <td id="T_f38f10e2_2afc_11e8_919e_9a00ccc28801row5_col0" class="data row5 col0" >8.20%</td> 
        <td id="T_f38f10e2_2afc_11e8_919e_9a00ccc28801row5_col1" class="data row5 col1" >47</td> 
    </tr>    <tr> 
        <th id="T_f38f10e2_2afc_11e8_919e_9a00ccc28801level0_row6" class="row_heading level0 row6" >35-39</th> 
        <td id="T_f38f10e2_2afc_11e8_919e_9a00ccc28801row6_col0" class="data row6 col0" >4.71%</td> 
        <td id="T_f38f10e2_2afc_11e8_919e_9a00ccc28801row6_col1" class="data row6 col1" >27</td> 
    </tr>    <tr> 
        <th id="T_f38f10e2_2afc_11e8_919e_9a00ccc28801level0_row7" class="row_heading level0 row7" >40-44</th> 
        <td id="T_f38f10e2_2afc_11e8_919e_9a00ccc28801row7_col0" class="data row7 col0" >1.75%</td> 
        <td id="T_f38f10e2_2afc_11e8_919e_9a00ccc28801row7_col1" class="data row7 col1" >10</td> 
    </tr>    <tr> 
        <th id="T_f38f10e2_2afc_11e8_919e_9a00ccc28801level0_row8" class="row_heading level0 row8" >45-49</th> 
        <td id="T_f38f10e2_2afc_11e8_919e_9a00ccc28801row8_col0" class="data row8 col0" >0.17%</td> 
        <td id="T_f38f10e2_2afc_11e8_919e_9a00ccc28801row8_col1" class="data row8 col1" >1</td> 
    </tr>    <tr> 
        <th id="T_f38f10e2_2afc_11e8_919e_9a00ccc28801level0_row9" class="row_heading level0 row9" >>50</th> 
        <td id="T_f38f10e2_2afc_11e8_919e_9a00ccc28801row9_col0" class="data row9 col0" >0.00%</td> 
        <td id="T_f38f10e2_2afc_11e8_919e_9a00ccc28801row9_col1" class="data row9 col1" >0</td> 
    </tr></tbody> 
</table> 




```python
age_grouped_df = pd.DataFrame({"Average Price":age_avg_price,
                               "Total Purchases":age_total_purchases,
                               "Normalized Total":age_revenue_norm})
age_grouped_df = age_grouped_df.dropna()
age_grouped_df.style.format({"Average Price":"${:,.2f}","Normalized Total":"${:,.2f}"})

```




<style  type="text/css" >
</style>  
<table id="T_f3e00cb8_2afc_11e8_8fa9_9a00ccc28801" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Average Price</th> 
        <th class="col_heading level0 col1" >Normalized Total</th> 
        <th class="col_heading level0 col2" >Total Purchases</th> 
    </tr>    <tr> 
        <th class="index_name level0" >Age Range</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_f3e00cb8_2afc_11e8_8fa9_9a00ccc28801level0_row0" class="row_heading level0 row0" ><10</th> 
        <td id="T_f3e00cb8_2afc_11e8_8fa9_9a00ccc28801row0_col0" class="data row0 col0" >$2.98</td> 
        <td id="T_f3e00cb8_2afc_11e8_8fa9_9a00ccc28801row0_col1" class="data row0 col1" >$4.39</td> 
        <td id="T_f3e00cb8_2afc_11e8_8fa9_9a00ccc28801row0_col2" class="data row0 col2" >28</td> 
    </tr>    <tr> 
        <th id="T_f3e00cb8_2afc_11e8_8fa9_9a00ccc28801level0_row1" class="row_heading level0 row1" >10-14</th> 
        <td id="T_f3e00cb8_2afc_11e8_8fa9_9a00ccc28801row1_col0" class="data row1 col0" >$2.77</td> 
        <td id="T_f3e00cb8_2afc_11e8_8fa9_9a00ccc28801row1_col1" class="data row1 col1" >$4.22</td> 
        <td id="T_f3e00cb8_2afc_11e8_8fa9_9a00ccc28801row1_col2" class="data row1 col2" >35</td> 
    </tr>    <tr> 
        <th id="T_f3e00cb8_2afc_11e8_8fa9_9a00ccc28801level0_row2" class="row_heading level0 row2" >15-19</th> 
        <td id="T_f3e00cb8_2afc_11e8_8fa9_9a00ccc28801row2_col0" class="data row2 col0" >$2.91</td> 
        <td id="T_f3e00cb8_2afc_11e8_8fa9_9a00ccc28801row2_col1" class="data row2 col1" >$3.86</td> 
        <td id="T_f3e00cb8_2afc_11e8_8fa9_9a00ccc28801row2_col2" class="data row2 col2" >133</td> 
    </tr>    <tr> 
        <th id="T_f3e00cb8_2afc_11e8_8fa9_9a00ccc28801level0_row3" class="row_heading level0 row3" >20-24</th> 
        <td id="T_f3e00cb8_2afc_11e8_8fa9_9a00ccc28801row3_col0" class="data row3 col0" >$2.91</td> 
        <td id="T_f3e00cb8_2afc_11e8_8fa9_9a00ccc28801row3_col1" class="data row3 col1" >$3.78</td> 
        <td id="T_f3e00cb8_2afc_11e8_8fa9_9a00ccc28801row3_col2" class="data row3 col2" >336</td> 
    </tr>    <tr> 
        <th id="T_f3e00cb8_2afc_11e8_8fa9_9a00ccc28801level0_row4" class="row_heading level0 row4" >25-29</th> 
        <td id="T_f3e00cb8_2afc_11e8_8fa9_9a00ccc28801row4_col0" class="data row4 col0" >$2.96</td> 
        <td id="T_f3e00cb8_2afc_11e8_8fa9_9a00ccc28801row4_col1" class="data row4 col1" >$4.26</td> 
        <td id="T_f3e00cb8_2afc_11e8_8fa9_9a00ccc28801row4_col2" class="data row4 col2" >125</td> 
    </tr>    <tr> 
        <th id="T_f3e00cb8_2afc_11e8_8fa9_9a00ccc28801level0_row5" class="row_heading level0 row5" >30-34</th> 
        <td id="T_f3e00cb8_2afc_11e8_8fa9_9a00ccc28801row5_col0" class="data row5 col0" >$3.08</td> 
        <td id="T_f3e00cb8_2afc_11e8_8fa9_9a00ccc28801row5_col1" class="data row5 col1" >$4.20</td> 
        <td id="T_f3e00cb8_2afc_11e8_8fa9_9a00ccc28801row5_col2" class="data row5 col2" >64</td> 
    </tr>    <tr> 
        <th id="T_f3e00cb8_2afc_11e8_8fa9_9a00ccc28801level0_row6" class="row_heading level0 row6" >35-39</th> 
        <td id="T_f3e00cb8_2afc_11e8_8fa9_9a00ccc28801row6_col0" class="data row6 col0" >$2.84</td> 
        <td id="T_f3e00cb8_2afc_11e8_8fa9_9a00ccc28801row6_col1" class="data row6 col1" >$4.42</td> 
        <td id="T_f3e00cb8_2afc_11e8_8fa9_9a00ccc28801row6_col2" class="data row6 col2" >42</td> 
    </tr>    <tr> 
        <th id="T_f3e00cb8_2afc_11e8_8fa9_9a00ccc28801level0_row7" class="row_heading level0 row7" >40-44</th> 
        <td id="T_f3e00cb8_2afc_11e8_8fa9_9a00ccc28801row7_col0" class="data row7 col0" >$3.19</td> 
        <td id="T_f3e00cb8_2afc_11e8_8fa9_9a00ccc28801row7_col1" class="data row7 col1" >$5.10</td> 
        <td id="T_f3e00cb8_2afc_11e8_8fa9_9a00ccc28801row7_col2" class="data row7 col2" >16</td> 
    </tr>    <tr> 
        <th id="T_f3e00cb8_2afc_11e8_8fa9_9a00ccc28801level0_row8" class="row_heading level0 row8" >45-49</th> 
        <td id="T_f3e00cb8_2afc_11e8_8fa9_9a00ccc28801row8_col0" class="data row8 col0" >$2.72</td> 
        <td id="T_f3e00cb8_2afc_11e8_8fa9_9a00ccc28801row8_col1" class="data row8 col1" >$2.72</td> 
        <td id="T_f3e00cb8_2afc_11e8_8fa9_9a00ccc28801row8_col2" class="data row8 col2" >1</td> 
    </tr></tbody> 
</table> 




```python
per_person_group = raw_data_df.groupby("SN")
total_spent_per_sn = per_person_group["Price"].sum()
number_of_purchases_per_sn = raw_data_df.groupby("SN").size()
avg_purchase_price_per_sn = total_spent_per_sn / number_of_purchases_per_sn
total_spent_per_sn_df = pd.DataFrame({"Number of Purchases":number_of_purchases_per_sn,
                                      "Total Purchase Value":total_spent_per_sn,
                                      "Average Purchase Price": avg_purchase_price_per_sn})
total_spent_per_sn_df = total_spent_per_sn_df.sort_values(by="Total Purchase Value", ascending=False)

#total_spent_per_sn_df.style.format({"Average Purchase Price":"${:,.2f}",
#                                    "Total Purchase Value":"${:,.2f}",})

total_spent_per_sn_df["Total Purchase Value"] = total_spent_per_sn_df["Total Purchase Value"].map('${:,.2f}'.format)
total_spent_per_sn_df["Average Purchase Price"] = total_spent_per_sn_df["Average Purchase Price"].map('${:,.2f}'.format)
total_spent_per_sn_df.head()
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
      <th>Average Purchase Price</th>
      <th>Number of Purchases</th>
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
      <td>$3.41</td>
      <td>5</td>
      <td>$17.06</td>
    </tr>
    <tr>
      <th>Saedue76</th>
      <td>$3.39</td>
      <td>4</td>
      <td>$13.56</td>
    </tr>
    <tr>
      <th>Mindimnya67</th>
      <td>$3.18</td>
      <td>4</td>
      <td>$12.74</td>
    </tr>
    <tr>
      <th>Haellysu29</th>
      <td>$4.24</td>
      <td>3</td>
      <td>$12.73</td>
    </tr>
    <tr>
      <th>Eoda93</th>
      <td>$3.86</td>
      <td>3</td>
      <td>$11.58</td>
    </tr>
  </tbody>
</table>
</div>




```python
by_item_group = raw_data_df.groupby("Item ID")
total_revenue_per_item = by_item_group["Price"].sum()
number_of_item_sold = by_item_group["Item ID"].size()
item_name = by_item_group["Item Name"].first()
price_of_item = by_item_group["Price"]

popular_items_df = pd.DataFrame({"Item Name":item_name,
                                    "Total Purchase Value": total_revenue_per_item,
                                    "Number Sold":number_of_item_sold})
popular_items_df = popular_items_df.sort_values(by="Number Sold", ascending=False)
profitable_items_df = popular_items_df.sort_values(by="Total Purchase Value", ascending=False)
popular_items_df["Total Purchase Value"] = popular_items_df["Total Purchase Value"].map('${:,.2f}'.format)

popular_items_df.head()
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
      <th>Number Sold</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Item ID</th>
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
      <td>$25.85</td>
    </tr>
    <tr>
      <th>84</th>
      <td>Arcane Gem</td>
      <td>11</td>
      <td>$24.53</td>
    </tr>
    <tr>
      <th>31</th>
      <td>Trickster</td>
      <td>9</td>
      <td>$18.63</td>
    </tr>
    <tr>
      <th>175</th>
      <td>Woeful Adamantite Claymore</td>
      <td>9</td>
      <td>$11.16</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Serenity</td>
      <td>9</td>
      <td>$13.41</td>
    </tr>
  </tbody>
</table>
</div>




```python
profitable_items_df["Total Purchase Value"] = profitable_items_df["Total Purchase Value"].map('${:,.2f}'.format)
profitable_items_df.head()
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
      <th>Number Sold</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Item ID</th>
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
      <td>$37.26</td>
    </tr>
    <tr>
      <th>115</th>
      <td>Spectral Diamond Doomblade</td>
      <td>7</td>
      <td>$29.75</td>
    </tr>
    <tr>
      <th>32</th>
      <td>Orenmir</td>
      <td>6</td>
      <td>$29.70</td>
    </tr>
    <tr>
      <th>103</th>
      <td>Singed Scalpel</td>
      <td>6</td>
      <td>$29.22</td>
    </tr>
    <tr>
      <th>107</th>
      <td>Splitter, Foe Of Subtlety</td>
      <td>8</td>
      <td>$28.88</td>
    </tr>
  </tbody>
</table>
</div>


