# CS304 Unit #7 Discussion Notebook

Use this notebook to complete discussion #7 instructions

Student Name: Christopher Holt

## 1. Initial Post

Your assigned number is 7

Create a DataFrame below with your assigned number replacing the value in the third line's **nGenerator** variable:



```python
import pandas as pd
import numpy as np

nGenerator = 7   # Place your assigned number instead of 10 
rng = np.random.RandomState(nGenerator)
nItems = 6
nTransactions = 20
maxPurchases = 5
allItems =[]
allTransactions =  ["T"+str(i) for i in range(1,nTransactions+1)]
for i in range(nTransactions):
    nPurchases = rng.randint(3,maxPurchases+1)
    itemset = ()
    i = 0
    while len(itemset)<nPurchases:
        itemset = set(rng.randint(1,nItems+1,size=nPurchases))
    items = list(itemset)
    items.sort()
    items = ["I"+str(i) for i in items]
    allItems.append(items) 
Transactions = pd.Series(allItems, index=allTransactions, name="Items Purchased")
Transactions = Transactions.to_frame().rename_axis("Transactions")
Transactions
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
      <th>Items Purchased</th>
    </tr>
    <tr>
      <th>Transactions</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>T1</th>
      <td>[I1, I2, I5]</td>
    </tr>
    <tr>
      <th>T2</th>
      <td>[I1, I2, I3, I5]</td>
    </tr>
    <tr>
      <th>T3</th>
      <td>[I1, I3, I4, I5, I6]</td>
    </tr>
    <tr>
      <th>T4</th>
      <td>[I2, I3, I5, I6]</td>
    </tr>
    <tr>
      <th>T5</th>
      <td>[I1, I3, I5]</td>
    </tr>
    <tr>
      <th>T6</th>
      <td>[I1, I2, I3, I4, I6]</td>
    </tr>
    <tr>
      <th>T7</th>
      <td>[I1, I2, I3, I4, I6]</td>
    </tr>
    <tr>
      <th>T8</th>
      <td>[I1, I5, I6]</td>
    </tr>
    <tr>
      <th>T9</th>
      <td>[I2, I4, I6]</td>
    </tr>
    <tr>
      <th>T10</th>
      <td>[I1, I2, I4, I6]</td>
    </tr>
    <tr>
      <th>T11</th>
      <td>[I1, I2, I3, I4]</td>
    </tr>
    <tr>
      <th>T12</th>
      <td>[I2, I4, I6]</td>
    </tr>
    <tr>
      <th>T13</th>
      <td>[I1, I2, I4, I5]</td>
    </tr>
    <tr>
      <th>T14</th>
      <td>[I1, I2, I6]</td>
    </tr>
    <tr>
      <th>T15</th>
      <td>[I1, I3, I4, I5, I6]</td>
    </tr>
    <tr>
      <th>T16</th>
      <td>[I1, I5, I6]</td>
    </tr>
    <tr>
      <th>T17</th>
      <td>[I2, I3, I5, I6]</td>
    </tr>
    <tr>
      <th>T18</th>
      <td>[I1, I2, I3, I4, I6]</td>
    </tr>
    <tr>
      <th>T19</th>
      <td>[I1, I2, I3, I4]</td>
    </tr>
    <tr>
      <th>T20</th>
      <td>[I1, I2, I3, I4, I6]</td>
    </tr>
  </tbody>
</table>
</div>




```python
from mlxtend.frequent_patterns import apriori

apriori(df, min_support=0.25, use_colnames=True)
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
      <th>support</th>
      <th>itemsets</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0.80</td>
      <td>(I1)</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0.75</td>
      <td>(I2)</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0.60</td>
      <td>(I3)</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0.60</td>
      <td>(I4)</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0.50</td>
      <td>(I5)</td>
    </tr>
    <tr>
      <th>5</th>
      <td>0.70</td>
      <td>(I6)</td>
    </tr>
    <tr>
      <th>6</th>
      <td>0.55</td>
      <td>(I1, I2)</td>
    </tr>
    <tr>
      <th>7</th>
      <td>0.50</td>
      <td>(I3, I1)</td>
    </tr>
    <tr>
      <th>8</th>
      <td>0.50</td>
      <td>(I4, I1)</td>
    </tr>
    <tr>
      <th>9</th>
      <td>0.40</td>
      <td>(I5, I1)</td>
    </tr>
    <tr>
      <th>10</th>
      <td>0.50</td>
      <td>(I1, I6)</td>
    </tr>
    <tr>
      <th>11</th>
      <td>0.45</td>
      <td>(I3, I2)</td>
    </tr>
    <tr>
      <th>12</th>
      <td>0.50</td>
      <td>(I4, I2)</td>
    </tr>
    <tr>
      <th>13</th>
      <td>0.25</td>
      <td>(I5, I2)</td>
    </tr>
    <tr>
      <th>14</th>
      <td>0.50</td>
      <td>(I2, I6)</td>
    </tr>
    <tr>
      <th>15</th>
      <td>0.40</td>
      <td>(I4, I3)</td>
    </tr>
    <tr>
      <th>16</th>
      <td>0.30</td>
      <td>(I5, I3)</td>
    </tr>
    <tr>
      <th>17</th>
      <td>0.40</td>
      <td>(I3, I6)</td>
    </tr>
    <tr>
      <th>18</th>
      <td>0.45</td>
      <td>(I4, I6)</td>
    </tr>
    <tr>
      <th>19</th>
      <td>0.30</td>
      <td>(I5, I6)</td>
    </tr>
    <tr>
      <th>20</th>
      <td>0.35</td>
      <td>(I3, I1, I2)</td>
    </tr>
    <tr>
      <th>21</th>
      <td>0.40</td>
      <td>(I4, I1, I2)</td>
    </tr>
    <tr>
      <th>22</th>
      <td>0.30</td>
      <td>(I1, I2, I6)</td>
    </tr>
    <tr>
      <th>23</th>
      <td>0.40</td>
      <td>(I4, I3, I1)</td>
    </tr>
    <tr>
      <th>24</th>
      <td>0.30</td>
      <td>(I3, I1, I6)</td>
    </tr>
    <tr>
      <th>25</th>
      <td>0.35</td>
      <td>(I4, I1, I6)</td>
    </tr>
    <tr>
      <th>26</th>
      <td>0.30</td>
      <td>(I4, I3, I2)</td>
    </tr>
    <tr>
      <th>27</th>
      <td>0.30</td>
      <td>(I3, I2, I6)</td>
    </tr>
    <tr>
      <th>28</th>
      <td>0.35</td>
      <td>(I4, I2, I6)</td>
    </tr>
    <tr>
      <th>29</th>
      <td>0.30</td>
      <td>(I4, I3, I6)</td>
    </tr>
    <tr>
      <th>30</th>
      <td>0.30</td>
      <td>(I4, I3, I1, I2)</td>
    </tr>
    <tr>
      <th>31</th>
      <td>0.25</td>
      <td>(I4, I1, I2, I6)</td>
    </tr>
    <tr>
      <th>32</th>
      <td>0.30</td>
      <td>(I4, I3, I1, I6)</td>
    </tr>
  </tbody>
</table>
</div>




```python
from mlxtend.frequent_patterns import apriori

# Find the number of transactions
num_transactions = len(df)

# Calculate min_support as a proportion
min_support = 5 / num_transactions

# Apply the apriori algorithm
frequent_itemsets = apriori(df, min_support=min_support, use_colnames=True)

print(frequent_itemsets)
```

        support          itemsets
    0      0.80              (I1)
    1      0.75              (I2)
    2      0.60              (I3)
    3      0.60              (I4)
    4      0.50              (I5)
    5      0.70              (I6)
    6      0.55          (I1, I2)
    7      0.50          (I3, I1)
    8      0.50          (I4, I1)
    9      0.40          (I5, I1)
    10     0.50          (I1, I6)
    11     0.45          (I3, I2)
    12     0.50          (I4, I2)
    13     0.25          (I5, I2)
    14     0.50          (I2, I6)
    15     0.40          (I4, I3)
    16     0.30          (I5, I3)
    17     0.40          (I3, I6)
    18     0.45          (I4, I6)
    19     0.30          (I5, I6)
    20     0.35      (I3, I1, I2)
    21     0.40      (I4, I1, I2)
    22     0.30      (I1, I2, I6)
    23     0.40      (I4, I3, I1)
    24     0.30      (I3, I1, I6)
    25     0.35      (I4, I1, I6)
    26     0.30      (I4, I3, I2)
    27     0.30      (I3, I2, I6)
    28     0.35      (I4, I2, I6)
    29     0.30      (I4, I3, I6)
    30     0.30  (I4, I3, I1, I2)
    31     0.25  (I4, I1, I2, I6)
    32     0.30  (I4, I3, I1, I6)
    


```python
frequent_itemsets['k'] = frequent_itemsets['itemsets'].apply(len)
counts = frequent_itemsets['k'].value_counts().sort_index()

for k in range(1, 5):
    print(f"k{k}: {counts.get(k, 0)}")
```

    k1: 6
    k2: 14
    k3: 10
    k4: 3
    


```python

```
