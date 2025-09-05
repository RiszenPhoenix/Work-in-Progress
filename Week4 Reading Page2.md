```python
%matplotlib inline

import pandas as pd
import matplotlib
import matplotlib.pyplot as plt
import psycopg2
import seaborn as sbn
from altair import Chart, X, Y, Color, Scale
import altair as alt
from vega_datasets import data
import requests
from bs4 import BeautifulSoup
matplotlib.style.use('ggplot')
```


```python
ls C:\Users\risze\Downloads\factbook-2017_unzipped\factbook-2017
```

     Volume in drive C is Windows 
     Volume Serial Number is 1AB6-6ED2
    
     Directory of C:\Users\risze\Downloads\factbook-2017_unzipped\factbook-2017
    
    09/03/2025  02:01 PM    <DIR>          .
    09/03/2025  02:01 PM    <DIR>          ..
    09/03/2025  02:01 PM    <DIR>          appendix
    09/03/2025  02:01 PM    <DIR>          css
    09/03/2025  02:01 PM    <DIR>          docs
    09/03/2025  02:01 PM    <DIR>          fields
    09/03/2025  02:01 PM    <DIR>          fonts
    09/03/2025  02:01 PM    <DIR>          geos
    09/03/2025  02:01 PM    <DIR>          graphics
    09/03/2025  02:01 PM    <DIR>          images
    09/03/2025  02:01 PM            76,028 index.html
    09/03/2025  02:01 PM    <DIR>          js
    09/03/2025  02:01 PM    <DIR>          print
    09/03/2025  02:01 PM             4,594 print_Contact.pdf
    09/03/2025  02:01 PM    <DIR>          rankorder
    09/03/2025  02:01 PM    <DIR>          scripts
    09/03/2025  02:01 PM    <DIR>          styles
    09/03/2025  02:01 PM    <DIR>          wfbExt
                   2 File(s)         80,622 bytes
                  16 Dir(s)   9,811,525,632 bytes free
    


```python
import os
files = os.listdir(r"C:\Users\risze\Downloads\factbook-2017_unzipped\factbook-2017/fields")
print(sorted(files)[:10])
```

    ['2001.html', '2002.html', '2003.html', '2004.html', '2006.html', '2007.html', '2008.html', '2010.html', '2011.html', '2012.html']
    


```python
print(sorted(files['2001.html']))
```


    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    Cell In[18], line 1
    ----> 1 print(sorted(files['2001.html']))
    

    TypeError: list indices must be integers or slices, not str



```python
from bs4 import BeautifulSoup
from pathlib import Path

# file_path = Path(r'C:\Users\risze\Downloads\factbook-2017_unzipped\factbook-2017\docs\notesanddefs.html')
# page = file_path.read_text(encoding='utf-8', errors='replace')
with open(r'C:\Users\risze\Downloads\factbook-2017_unzipped\factbook-2017\docs\notesanddefs.html', 'rb') as f:
    page = f.read()
    # print(f)
# page = open(r'C:\Users\risze\Downloads\factbook-2017_unzipped\factbook-2017\docs\notesanddefs.html').read()
page[:200]
```




    b'<!doctype html>\r\n<!--[if lt IE 7]> <html class="no-js lt-ie9 lt-ie8 lt-ie7" lang="en"> <![endif]-->\r\n<!--[if IE 7]>    <html class="no-js lt-ie9 lt-ie8" lang="en"> <![endif]-->\r\n<!--[if IE 8]>    <htm'




```python
page = BeautifulSoup(page)
print(page.prettify()[:1000])
```

    <!DOCTYPE html>
    <!--[if lt IE 7]> <html class="no-js lt-ie9 lt-ie8 lt-ie7" lang="en"> <![endif]-->
    <!--[if IE 7]>    <html class="no-js lt-ie9 lt-ie8" lang="en"> <![endif]-->
    <!--[if IE 8]>    <html class="no-js lt-ie9" lang="en"> <![endif]-->
    <!--[if gt IE 8]><!-->
    <!--<![endif]-->
    <html class="no-js" lang="en">
     <!-- InstanceBegin template="/Templates/wfbext_template.dwt.cfm" codeOutsideHTMLIsLocked="false" -->
     <head>
      <meta charset="utf-8"/>
      <meta content="IE=edge,chrome=1" http-equiv="X-UA-Compatible"/>
      <!-- InstanceBeginEditable name="doctitle" -->
      <title>
       The World Factbook
      </title>
      <!-- InstanceEndEditable -->
      <meta content="" name="description"/>
      <meta content="width=device-width" name="viewport"/>
      <link href="../css/fullscreen-external.css" rel="stylesheet" type="text/css"/>
      <script src="../js/modernizr-latest.js">
      </script>
      <!--developers version - switch to specific production http://modernizr.com/download/-->
      <script src="../js/jquery-1.8.3.min.
    


```python
links = page.select('a')
print(len(links))
links[-1]
```

    625
    




    <a class="go-top" href="#">GO TOP</a>




```python
links = page.select('div')
print(len(links))
links[-1]
```

    793
    




    <div align="center" style="width: 100%;"><a href="/open/"><img alt="open gov" height="24" src="../images/ico-06.png" width="101"/></a></div>




```python
links = page.select('.cfclose')
print(len(links))
links[-1]
```

    3
    




    <button aria-hidden="true" aria-label="Close" class="cfclose" data-dismiss="modal" type="button">×</button>




```python
cols = page.select("span.category")
for col in cols:
    cells = col.select('td')
    col_name = cells[0].text
    print(col_name)
```

    Abbreviations
    Acronyms
    Administrative divisions
    Age structure
    Agriculture - products
    Airports
    Airports - with paved runways
    Airports - with unpaved runways
    Appendixes
    Area
    Area - comparative
    Background
    Birth rate
    Broadcast media
    Budget
    Budget surplus (+) or deficit (-)
    Capital
    Carbon dioxide emissions from consumption of energy
    Central bank discount rate
    Child labor - children ages 5-14
    Children under the age of 5 years underweight
    Citizenship
    Civil aircraft registration country code prefix
    Climate
    Coastline
    Commercial bank prime lending rate
    Communications
    Communications - note
    Constitution
    Contraceptive prevalence rate
    Coordinated Universal Time (UTC)
    Country data codes
    Country map
    Country name
    Crude oil - exports
    Crude oil - imports
    Crude oil - production
    Crude oil - proved reserves
    Current account balance
    Data codes
    Date of information
    Daylight Saving Time (DST)
    Death rate
    Debt - external
    Demographic profile
    Dependency ratios
    Dependency status
    Dependent areas
    Diplomatic representation
    Diplomatic representation from the US
    Diplomatic representation in the US
    Disputes - international
    Distribution of family income - Gini index
    Drinking water source
    Economy
    Economy - overview
    Education expenditures
    Electricity - access
    Electricity - consumption
    Electricity - exports
    Electricity - from fossil fuels
    Electricity - from hydroelectric plants
    Electricity - from nuclear fuels
    Electricity - from other renewable sources
    Electricity - imports
    Electricity - installed generating capacity
    Electricity - production
    Elevation
    Energy
    Entities
    Environment - current issues
    Environment - international agreements
    Environmental agreements
    Ethnic groups
    Exchange rates
    Executive branch
    Exports
    Exports - commodities
    Exports - partners
    Fiscal year
    Flag description
    Flag graphic
    GDP (official exchange rate)
    GDP (purchasing power parity)
    GDP - composition, by end use
    GDP - composition, by sector of origin
    GDP - per capita (PPP)
    GDP - real growth rate
    GDP methodology
    Geographic coordinates
    Geographic names
    Geography
    Geography - note
    Gini index
    GNP
    Government
    Government - note
    Government type
    Greenwich Mean Time (GMT)
    Gross domestic product
    Gross national product
    Gross national saving
    Gross world product
    GWP
    Health expenditures
    Heliports
    HIV/AIDS - adult prevalence rate
    HIV/AIDS - deaths
    HIV/AIDS - people living with HIV/AIDS
    Hospital bed density
    Household income or consumption by percentage share
    Hydrographic data codes
    Illicit drugs
    Imports
    Imports - commodities
    Imports - partners
    Independence
    Industrial production growth rate
    Industries
    Infant mortality rate
    Inflation rate (consumer prices)
    International disputes
    International law organization participation
    International organization participation
    International organizations
    Internet country code
    Internet users
    Introduction
    Investment (gross fixed)
    Irrigated land
    Judicial branch
    Labor force
    Labor force - by occupation
    Land boundaries
    Land use
    Languages
    Legal system
    Legislative branch
    Life expectancy at birth
    Literacy
    Location
    Major infectious diseases
    Major urban areas - population
    Map references
    Maritime claims
    Market value of publicly traded shares
    Maternal mortality rate
    Median age
    Merchant marine
    Military
    Military - note
    Military branches
    Military expenditures
    Military service age and obligation
    Money figures
    Mother's mean age at first birth
    National air transport system
    National anthem
    National holiday
    National symbol(s)
    Nationality
    Natural gas - consumption
    Natural gas - exports
    Natural gas - imports
    Natural gas - production
    Natural gas - proved reserves
    Natural hazards
    Natural resources
    Net migration rate
    Obesity - adult prevalence rate
    People - note
    People and Society
    Personal Names - Capitalization
    Personal Names - Spelling
    Personal Names - Titles
    Petroleum
    Petroleum products
    Physicians density
    Pipelines
    Piracy
    Political parties and leaders
    Political pressure groups and leaders
    Population
    Population below poverty line
    Population distribution
    Population growth rate
    Population pyramid
    Ports and terminals
    Principality
    Public debt
    Railways
    Rare earth elements
    Reference maps
    Refined petroleum products - consumption
    Refined petroleum products - exports
    Refined petroleum products - imports
    Refined petroleum products - production
    Refugees and internally displaced persons
    Religions
    Reserves of foreign exchange and gold
    Roadways
    Sanitation facility access
    School life expectancy (primary to tertiary education)
    Sex ratio
    Stateless person
    Stock of broad money
    Stock of direct foreign investment - abroad
    Stock of direct foreign investment - at home
    Stock of domestic credit
    Stock of narrow money
    Suffrage
    Taxes and other revenues
    Telephone numbers
    Telephone system
    Telephones - fixed lines
    Telephones - mobile cellular
    Terminology
    Terrain
    Time difference
    Time zones
    Total fertility rate
    Trafficking in persons
    Transnational issues
    Transportation
    Transportation - note
    Unemployment rate
    Unemployment, youth ages 15-24
    Urbanization
    UTC (Coordinated Universal Time)
    Waterways
    Weights and Measures
    Years
    


```python
cols = page.select("span.category")
for col in cols:
    cells = col.select('td')
    colname = cells[0].text
    links = cells[1].select('a')
    if len(links) > 0:
        fpath = links[0]['href']
        print(colname, fpath)
```

    Administrative divisions ../fields/2051.html#3
    Age structure ../fields/2010.html#4
    Agriculture - products ../fields/2052.html#5
    Airports ../fields/2053.html#6
    Airports - with paved runways ../fields/2030.html#7
    Airports - with unpaved runways ../fields/2031.html#8
    Area ../fields/2147.html#10
    Area - comparative ../fields/2023.html#11
    Background ../fields/2028.html#12
    Birth rate ../fields/2054.html#13
    Broadcast media ../fields/2213.html#14
    Budget ../fields/2056.html#15
    Budget surplus (+) or deficit (-) ../fields/2222.html#16
    Capital ../fields/2057.html#17
    Carbon dioxide emissions from consumption of energy ../fields/2254.html#18
    Central bank discount rate ../fields/2207.html#19
    Child labor - children ages 5-14 ../fields/2255.html#20
    Children under the age of 5 years underweight ../fields/2224.html#21
    Citizenship ../fields/2263.html
    Civil aircraft registration country code prefix ../fields/2270.html
    Climate ../fields/2059.html#22
    Coastline ../fields/2060.html#23
    Commercial bank prime lending rate ../fields/2208.html#24
    Communications - note ../fields/2138.html#26
    Constitution ../fields/2063.html#27
    Contraceptive prevalence rate ../fields/2258.html#28
    Country name ../fields/2142.html#32
    Crude oil - exports ../fields/2242.html#33
    Crude oil - imports ../fields/2243.html#34
    Crude oil - production ../fields/2241.html#35
    Crude oil - proved reserves ../fields/2244.html#36
    Current account balance ../fields/2187.html#37
    Death rate ../fields/2066.html#41
    Debt - external ../fields/2079.html#42
    Demographic profile ../fields/2257.html#43
    Dependency ratios ../fields/2261.html#44
    Dependency status ../fields/2006.html#45
    Dependent areas ../fields/2068.html#46
    Diplomatic representation from the US ../fields/2007.html#48
    Diplomatic representation in the US ../fields/2149.html#49
    Disputes - international ../fields/2070.html#50
    Distribution of family income - Gini index ../fields/2172.html#51
    Drinking water source ../fields/2216.html#52
    Economy - overview ../fields/2116.html#54
    Education expenditures ../fields/2206.html#55
    Electricity - access ../fields/2268.html
    Electricity - consumption ../fields/2233.html#56
    Electricity - exports ../fields/2234.html#57
    Electricity - from fossil fuels ../fields/2237.html#58
    Electricity - from hydroelectric plants ../fields/2238.html#59
    Electricity - from nuclear fuels ../fields/2239.html#60
    Electricity - from other renewable sources ../fields/2240.html#61
    Electricity - imports ../fields/2235.html#62
    Electricity - installed generating capacity ../fields/2236.html#63
    Electricity - production ../fields/2232.html#64
    Elevation ../fields/2020.html#65
    Environment - current issues ../fields/2032.html#68
    Environment - international agreements ../fields/2033.html#69
    Ethnic groups ../fields/2075.html#71
    Exchange rates ../fields/2076.html#72
    Executive branch ../fields/2077.html#73
    Exports ../fields/2078.html#74
    Exports - commodities ../fields/2049.html#75
    Exports - partners ../fields/2050.html#76
    Fiscal year ../fields/2080.html#77
    Flag description ../fields/2081.html#78
    GDP (official exchange rate) ../fields/2195.html#81
    GDP (purchasing power parity) ../fields/2001.html#82
    GDP - composition, by end use ../fields/2259.html#83
    GDP - composition, by sector of origin ../fields/2012.html#84
    GDP - per capita (PPP) ../fields/2004.html#85
    GDP - real growth rate ../fields/2003.html#86
    Geographic coordinates ../fields/2011.html#88
    Geography - note ../fields/2113.html#91
    Government - note ../fields/2140.html#95
    Government type ../fields/2128.html#96
    Gross national saving ../fields/2260.html#100
    Health expenditures ../fields/2225.html#103
    Heliports ../fields/2019.html#104
    HIV/AIDS - adult prevalence rate ../fields/2155.html#105
    HIV/AIDS - deaths ../fields/2157.html#106
    HIV/AIDS - people living with HIV/AIDS ../fields/2156.html#107
    Hospital bed density ../fields/2227.html#108
    Household income or consumption by percentage share ../fields/2047.html#109
    Illicit drugs ../fields/2086.html#111
    Imports ../fields/2087.html#112
    Imports - commodities ../fields/2058.html#113
    Imports - partners ../fields/2061.html#114
    Independence ../fields/2088.html#115
    Industrial production growth rate ../fields/2089.html#116
    Industries ../fields/2090.html#117
    Infant mortality rate ../fields/2091.html#118
    Inflation rate (consumer prices) ../fields/2092.html#119
    International law organization participation ../fields/2220.html#121
    International organization participation ../fields/2107.html#122
    Internet country code ../fields/2154.html#124
    Internet users ../fields/2153.html#126
    Irrigated land ../fields/2146.html#129
    Judicial branch ../fields/2094.html#130
    Labor force ../fields/2095.html#131
    Labor force - by occupation ../fields/2048.html#132
    Land boundaries ../fields/2096.html#133
    Land use ../fields/2097.html#134
    Languages ../fields/2098.html#135
    Legal system ../fields/2100.html#136
    Legislative branch ../fields/2101.html#137
    Life expectancy at birth ../fields/2102.html#138
    Literacy ../fields/2103.html#139
    Location ../fields/2144.html#140
    Major infectious diseases ../fields/2193.html#141
    Major urban areas - population ../fields/2219.html#142
    Map references ../fields/2145.html#146
    Maritime claims ../fields/2106.html#147
    Market value of publicly traded shares ../fields/2200.html#148
    Maternal mortality rate ../fields/2223.html#149
    Median age ../fields/2177.html#150
    Merchant marine ../fields/2108.html#151
    Military - note ../fields/2137.html#153
    Military branches ../fields/2055.html#154
    Military expenditures ../fields/2034.html#155
    Military service age and obligation ../fields/2024.html#156
    Mother's mean age at first birth ../fields/2256.html#158
    National air transport system ../fields/2269.html
    National anthem ../fields/2218.html#159
    National holiday ../fields/2109.html#160
    National symbol(s) ../fields/2230.html#161
    Nationality ../fields/2110.html#162
    Natural gas - consumption ../fields/2250.html#163
    Natural gas - exports ../fields/2251.html#164
    Natural gas - imports ../fields/2252.html#165
    Natural gas - production ../fields/2249.html#166
    Natural gas - proved reserves ../fields/2253.html#167
    Natural hazards ../fields/2021.html#168
    Natural resources ../fields/2111.html#169
    Net migration rate ../fields/2112.html#170
    Obesity - adult prevalence rate ../fields/2228.html#171
    People - note ../fields/2022.html#172
    Physicians density ../fields/2226.html#179
    Pipelines ../fields/2117.html#180
    Political parties and leaders ../fields/2118.html#182
    Political pressure groups and leaders ../fields/2115.html#183
    Population ../fields/2119.html#184
    Population below poverty line ../fields/2046.html#185
    Population distribution ../fields/2267.html
    Population growth rate ../fields/2002.html#186
    Ports and terminals ../fields/2120.html#188
    Public debt ../fields/2186.html#189
    Railways ../fields/2121.html#190
    Refined petroleum products - consumption ../fields/2246.html#193
    Refined petroleum products - exports ../fields/2247.html#194
    Refined petroleum products - imports ../fields/2248.html#195
    Refined petroleum products - production ../fields/2245.html#196
    Refugees and internally displaced persons ../fields/2194.html#197
    Religions ../fields/2122.html#198
    Reserves of foreign exchange and gold ../fields/2188.html#199
    Roadways ../fields/2085.html#200
    Sanitation facility access ../fields/2217.html#201
    School life expectancy (primary to tertiary education) ../fields/2205.html#202
    Sex ratio ../fields/2018.html#203
    Stock of broad money ../fields/2215.html#205
    Stock of direct foreign investment - abroad ../fields/2199.html#206
    Stock of direct foreign investment - at home ../fields/2198.html#207
    Stock of domestic credit ../fields/2211.html#208
    Stock of narrow money ../fields/2214.html#209
    Suffrage ../fields/2123.html#210
    Taxes and other revenues ../fields/2221.html#211
    Telephone system ../fields/2124.html#213
    Telephones - fixed lines ../fields/2150.html#214
    Telephones - mobile cellular ../fields/2151.html#215
    Terrain ../fields/2125.html#217
    Total fertility rate ../fields/2127.html#220
    Trafficking in persons ../fields/2196.html#222
    Transportation - note ../fields/2008.html#225
    Unemployment rate ../fields/2129.html#226
    Unemployment, youth ages 15-24 ../fields/2229.html#227
    Urbanization ../fields/2212.html#228
    Waterways ../fields/2093.html#230
    


```python
test = {
   # 'country_code', 'code2', 'code3', 'codeNum', 
     'country name', 'population', 'area', 'coastline', 'climate', 'net migration rate', 'birth rate', 'death rate', 'infant mortality rate', 
    'literacy', 'gdp (purchasing power parity)', 'government type', 'inflation rate (consumer prices)', 'health expenditures',
    'gdp - composition, by sector of origin' , 'land use', 'internet users'}

cols = page.select("span.category")
newlist=[]
for col in cols:
    cells = col.select('td')
    colname = cells[0].text
    links = cells[1].select('a')
    # if (colname == "Internet users"):
    if colname.lower() in test:
        if len(links) > 0:
            fpath = links[0]['href']
            newlist.append(fpath)
            print(fpath)
print(newlist)
```

    ../fields/2147.html#10
    ../fields/2054.html#13
    ../fields/2059.html#22
    ../fields/2060.html#23
    ../fields/2142.html#32
    ../fields/2066.html#41
    ../fields/2001.html#82
    ../fields/2012.html#84
    ../fields/2128.html#96
    ../fields/2225.html#103
    ../fields/2091.html#118
    ../fields/2092.html#119
    ../fields/2153.html#126
    ../fields/2097.html#134
    ../fields/2103.html#139
    ../fields/2112.html#170
    ../fields/2119.html#184
    ['../fields/2147.html#10', '../fields/2054.html#13', '../fields/2059.html#22', '../fields/2060.html#23', '../fields/2142.html#32', '../fields/2066.html#41', '../fields/2001.html#82', '../fields/2012.html#84', '../fields/2128.html#96', '../fields/2225.html#103', '../fields/2091.html#118', '../fields/2092.html#119', '../fields/2153.html#126', '../fields/2097.html#134', '../fields/2103.html#139', '../fields/2112.html#170', '../fields/2119.html#184']
    


```python
import os

# Set the base directory
base_dir = r'C:\Users\risze\Downloads\factbook-2017_unzipped\factbook2017\downloads'
#file:///C:/Users/risze/Downloads/factbook-2017_unzipped/factbook-2017/fields/print_2053.html

# Filter links that start with ..
c_drive_files = [link for link in newlist if link.startswith('')]

# Open each file
for filepath in c_drive_files:
    try:
        # Join the relative path with the base directory
        abs_path = os.path.abspath(os.path.join(base_dir, filepath))
        
        # Remove the anchor part (#...) if present
        if '#' in abs_path:
            abs_path = abs_path.split('#')[0]
            
        # Check if file exists
        if os.path.exists(abs_path):
            # Open the file
            with open(abs_path, 'r') as file:
                content = file.read()
                print(f"Successfully opened: {abs_path}")
        else:
            print(f"File not found: {abs_path}")
    except Exception as e:
        print(f"Error processing {filepath}: {str(e)}")
```

    File not found: C:\Users\risze\Downloads\factbook-2017_unzipped\factbook2017\fields\2147.html
    File not found: C:\Users\risze\Downloads\factbook-2017_unzipped\factbook2017\fields\2054.html
    File not found: C:\Users\risze\Downloads\factbook-2017_unzipped\factbook2017\fields\2059.html
    File not found: C:\Users\risze\Downloads\factbook-2017_unzipped\factbook2017\fields\2060.html
    File not found: C:\Users\risze\Downloads\factbook-2017_unzipped\factbook2017\fields\2142.html
    File not found: C:\Users\risze\Downloads\factbook-2017_unzipped\factbook2017\fields\2066.html
    File not found: C:\Users\risze\Downloads\factbook-2017_unzipped\factbook2017\fields\2001.html
    File not found: C:\Users\risze\Downloads\factbook-2017_unzipped\factbook2017\fields\2012.html
    File not found: C:\Users\risze\Downloads\factbook-2017_unzipped\factbook2017\fields\2128.html
    File not found: C:\Users\risze\Downloads\factbook-2017_unzipped\factbook2017\fields\2225.html
    File not found: C:\Users\risze\Downloads\factbook-2017_unzipped\factbook2017\fields\2091.html
    File not found: C:\Users\risze\Downloads\factbook-2017_unzipped\factbook2017\fields\2092.html
    File not found: C:\Users\risze\Downloads\factbook-2017_unzipped\factbook2017\fields\2153.html
    File not found: C:\Users\risze\Downloads\factbook-2017_unzipped\factbook2017\fields\2097.html
    File not found: C:\Users\risze\Downloads\factbook-2017_unzipped\factbook2017\fields\2103.html
    File not found: C:\Users\risze\Downloads\factbook-2017_unzipped\factbook2017\fields\2112.html
    File not found: C:\Users\risze\Downloads\factbook-2017_unzipped\factbook2017\fields\2119.html
    


```python
all_data = {'field name' : {coutry_code : value}, 'field name' : {country : value}, 'field name' : {code2 : value}, 'field name' : {code3 : value}, 
            'field name' : {CodeNum : value}, 'field name' : {Population, value}, 'field name' : {Area : value}, 'field name' : {Coastline : value}, 
            'field name' : {Climate : value}, 'field name' : {New_migration : value}, 'field name' : {Birth_rate : value}, 
            'field name' : {Death_rate : value}, 'field name' : {Infant_mortality_rate : value}, 'field name' : {Literacy : value}, 
            'field name' : {GDP : value}, 'field name' : {Government_Type : value}, 'field name' : {Inflation_rate : value}, 
            'field name' : {Health_expenditures : value}, 'field name' : {GDP_composition : value}, 'field name' : {Land_use : value}, 
            'field name' : {Internet_users : value}}
```


```python
all_data = {'Years' : {colname: fpath}}
print(all_data)
```

    {'Years': {'Years': '../fields/2119.html#184'}}
    


```python
import pandas as pd

# Create DataFrame from our all_data structure
table = pd.DataFrame.from_dict(all_data)

# Display first few rows
print(table.head())
```

                             Years
    Years  ../fields/2119.html#184
    


```python
pd.DataFrame(all_data)
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
      <th>Years</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Years</th>
      <td>../fields/2119.html#184</td>
    </tr>
  </tbody>
</table>
</div>




```python
print(all_data)
```

    {'Years': {'Years': '../fields/2119.html#184'}}
    


```python

```
