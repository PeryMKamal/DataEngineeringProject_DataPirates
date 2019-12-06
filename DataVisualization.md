```python
import pandas as pd
import datetime # to handle date/time attributes
from os import listdir # os is a module for interacting with the OS
from os.path import isfile, join # to verify file object, and concatenate paths
import glob # to find pathnames matching a specific pattern
import re # regular expressions :)
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

```


```python
NobelData = pd.read_csv("archiveData_Cleaned.csv")
NobelData
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
      <th>Unnamed: 0</th>
      <th>Year</th>
      <th>Category</th>
      <th>Prize</th>
      <th>Prize Share</th>
      <th>Laureate ID</th>
      <th>Laureate Type</th>
      <th>Full Name</th>
      <th>Birth Date</th>
      <th>Birth City</th>
      <th>Birth Country</th>
      <th>Sex</th>
      <th>Organization Name</th>
      <th>Organization City</th>
      <th>Organization Country</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>1901</td>
      <td>Chemistry</td>
      <td>The Nobel Prize in Chemistry 1901</td>
      <td>1/1</td>
      <td>160</td>
      <td>Individual</td>
      <td>Jacobus Henricus van 't Hoff</td>
      <td>1852-08-30</td>
      <td>Rotterdam</td>
      <td>Netherlands</td>
      <td>Male</td>
      <td>Berlin University</td>
      <td>Berlin</td>
      <td>Germany</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>1901</td>
      <td>Literature</td>
      <td>The Nobel Prize in Literature 1901</td>
      <td>1/1</td>
      <td>569</td>
      <td>Individual</td>
      <td>Sully Prudhomme</td>
      <td>1839-03-16</td>
      <td>Paris</td>
      <td>France</td>
      <td>Male</td>
      <td>Self</td>
      <td>Self</td>
      <td>Self</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>1901</td>
      <td>Medicine</td>
      <td>The Nobel Prize in Physiology or Medicine 1901</td>
      <td>1/1</td>
      <td>293</td>
      <td>Individual</td>
      <td>Emil Adolf von Behring</td>
      <td>1854-03-15</td>
      <td>Hansdorf (Lawice)</td>
      <td>Prussia (Poland)</td>
      <td>Male</td>
      <td>Marburg University</td>
      <td>Marburg</td>
      <td>Germany</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>1901</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 1901</td>
      <td>1/2</td>
      <td>462</td>
      <td>Individual</td>
      <td>Jean Henry Dunant</td>
      <td>1828-05-08</td>
      <td>Geneva</td>
      <td>Switzerland</td>
      <td>Male</td>
      <td>Self</td>
      <td>Self</td>
      <td>Self</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>1901</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 1901</td>
      <td>1/2</td>
      <td>463</td>
      <td>Individual</td>
      <td>Frédéric Passy</td>
      <td>1822-05-20</td>
      <td>Paris</td>
      <td>France</td>
      <td>Male</td>
      <td>Self</td>
      <td>Self</td>
      <td>Self</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>876</th>
      <td>964</td>
      <td>2016</td>
      <td>Medicine</td>
      <td>The Nobel Prize in Physiology or Medicine 2016</td>
      <td>1/1</td>
      <td>927</td>
      <td>Individual</td>
      <td>Yoshinori Ohsumi</td>
      <td>1945-02-09</td>
      <td>Fukuoka</td>
      <td>Japan</td>
      <td>Male</td>
      <td>Tokyo Institute of Technology</td>
      <td>Tokyo</td>
      <td>Japan</td>
    </tr>
    <tr>
      <th>877</th>
      <td>965</td>
      <td>2016</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 2016</td>
      <td>1/1</td>
      <td>934</td>
      <td>Individual</td>
      <td>Juan Manuel Santos</td>
      <td>1951-08-10</td>
      <td>Bogotá</td>
      <td>Colombia</td>
      <td>Male</td>
      <td>Self</td>
      <td>Self</td>
      <td>Self</td>
    </tr>
    <tr>
      <th>878</th>
      <td>966</td>
      <td>2016</td>
      <td>Physics</td>
      <td>The Nobel Prize in Physics 2016</td>
      <td>1/2</td>
      <td>928</td>
      <td>Individual</td>
      <td>David J. Thouless</td>
      <td>1934-09-21</td>
      <td>Bearsden</td>
      <td>United Kingdom</td>
      <td>Male</td>
      <td>University of Washington</td>
      <td>Seattle, WA</td>
      <td>United States of America</td>
    </tr>
    <tr>
      <th>879</th>
      <td>967</td>
      <td>2016</td>
      <td>Physics</td>
      <td>The Nobel Prize in Physics 2016</td>
      <td>1/4</td>
      <td>929</td>
      <td>Individual</td>
      <td>F. Duncan M. Haldane</td>
      <td>1951-09-14</td>
      <td>London</td>
      <td>United Kingdom</td>
      <td>Male</td>
      <td>Princeton University</td>
      <td>Princeton, NJ</td>
      <td>United States of America</td>
    </tr>
    <tr>
      <th>880</th>
      <td>968</td>
      <td>2016</td>
      <td>Physics</td>
      <td>The Nobel Prize in Physics 2016</td>
      <td>1/4</td>
      <td>930</td>
      <td>Individual</td>
      <td>J. Michael Kosterlitz</td>
      <td>1943-06-22</td>
      <td>Aberdeen</td>
      <td>United Kingdom</td>
      <td>Male</td>
      <td>Brown University</td>
      <td>Providence, RI</td>
      <td>United States of America</td>
    </tr>
  </tbody>
</table>
<p>881 rows × 15 columns</p>
</div>



## Creating data frames for each prize category for visualization


```python
Chemistry = NobelData[(NobelData.Category == 'Chemistry')]
Economics = NobelData[(NobelData.Category == 'Economics')]
Literature = NobelData[(NobelData.Category == 'Literature')]
Medicine = NobelData[(NobelData.Category == 'Medicine')]
Peace = NobelData[(NobelData.Category == 'Peace')]
Physics = NobelData[(NobelData.Category == 'Physics')]

```

## Nobel Prizes by Category from 1901 to 2016


```python
YearCategory=NobelData.groupby(['Year','Category'])['Laureate ID'].count().reset_index()
YearCategory
g = sns.FacetGrid(YearCategory, col='Category', hue='Category', col_wrap=8, )
g = g.map(plt.plot, 'Year', 'Laureate ID')
g = g.map(plt.fill_between, 'Year', 'Laureate ID', alpha=0.1).set_titles("{col_name} Category")
g = g.set_titles("{col_name}")
plt.show()
```


![png](DataVisualization_files/DataVisualization_5_0.png)


## Number of Nobel Prizes in each category


```python

NobelCategory=NobelData['Category'].value_counts()
sns.barplot(x=NobelCategory.index,y=NobelCategory.values)
plt.xticks(rotation=90)
plt.title('Nobel Prizes by Category')
plt.show()
```


![png](DataVisualization_files/DataVisualization_7_0.png)


## Visualization of number of prizes in each country in every category, USA dominance


```python

plt.figure(figsize=(6,22))
DataGraph = sns.countplot(y="Birth Country", data=NobelData,
              order=NobelData['Birth Country'].value_counts().index,
              palette='GnBu_d')
plt.show()
```


![png](DataVisualization_files/DataVisualization_9_0.png)


## Visualization of number of prizes in each country in Chemistry category


```python
plt.figure(figsize=(12,12))
ChemestryGraph = sns.countplot(y="Birth Country", data=Chemistry,
              order=Chemistry['Birth Country'].value_counts().index,
              palette='GnBu_d')
plt.show()
```


![png](DataVisualization_files/DataVisualization_11_0.png)


## Visualization of number of prizes in each country in Economics category


```python
plt.figure(figsize=(12,12))
EconomicsGraph = sns.countplot(y="Birth Country", data=Economics,
              order=Economics['Birth Country'].value_counts().index,
              palette='GnBu_d')
plt.show()
```


![png](DataVisualization_files/DataVisualization_13_0.png)


## Visualization of number of prizes in each country in Literature category


```python
plt.figure(figsize=(12,12))
LiteratureGraph = sns.countplot(y="Birth Country", data=Literature,
              order=Literature['Birth Country'].value_counts().index,
              palette='GnBu_d')
plt.show()
```


![png](DataVisualization_files/DataVisualization_15_0.png)


## Visualization of number of prizes in each country in Medicine category


```python
plt.figure(figsize=(12,12))
MedicineGraph = sns.countplot(y="Birth Country", data=Medicine,
              order=Medicine['Birth Country'].value_counts().index,
              palette='GnBu_d')
plt.show()
```


![png](DataVisualization_files/DataVisualization_17_0.png)


## Visualization of number of prizes in each country in Peace category


```python
plt.figure(figsize=(12,12))
PeaceGraph = sns.countplot(y="Birth Country", data=Peace,
              order=Peace['Birth Country'].value_counts().index,
              palette='GnBu_d')
plt.show()
```


![png](DataVisualization_files/DataVisualization_19_0.png)


## Visualization of number of prizes in each country in Physics category


```python
plt.figure(figsize=(12,12))
PhysicsGraph = sns.countplot(y="Birth Country", data=Physics,
              order=Physics['Birth Country'].value_counts().index,
              palette='GnBu_d')
plt.show()
```


![png](DataVisualization_files/DataVisualization_21_0.png)


## Pie chart for the whole dataframe gender, which gender is most likely to win a nobel prize?


```python
from matplotlib.pyplot import pie, axis, show

AllCategoryGender = NobelData['Sex'].value_counts()
print(AllCategoryGender)

pie(AllCategoryGender, labels=AllCategoryGender.index, autopct='%1.1f%%');
show()
```

    Male      833
    Female     48
    Name: Sex, dtype: int64
    


![png](DataVisualization_files/DataVisualization_23_1.png)


## Age differences between the whole nobel prize categories


```python
import warnings
warnings.filterwarnings('ignore')
NobelData['Birth Date'] = NobelData['Birth Date'].str[0:4]
NobelData['Birth Date'] = NobelData['Birth Date'].replace(to_replace="nan", value=0)
NobelData['Birth Date'] = NobelData['Birth Date'].apply(pd.to_numeric)
NobelData["Age"] = NobelData["Year"] - NobelData["Birth Date"]
```


```python
bins = [0, 19, 29, 39, 49, 59, 69, 79, 89, 100]
groupNames = ['Teens', '20s', '30s', '40s', '50s', '60s', '70s', '80s', '90s']
NobelData['Age Categorical'] = pd.cut(NobelData['Age'], bins, labels=groupNames)
```


```python
NobelAge = NobelData['Age Categorical'].value_counts()
print(NobelAge)



sns.barplot(x=NobelAge.index,y=NobelAge.values)
plt.xticks(rotation=90)
plt.title('Age differences')
plt.show()

```

    60s      248
    50s      228
    70s      155
    40s      155
    30s       48
    80s       44
    90s        1
    20s        1
    Teens      1
    Name: Age Categorical, dtype: int64
    


![png](DataVisualization_files/DataVisualization_27_1.png)


## Age differences between the chemistry category


```python
Chemistry['Birth Date'] = Chemistry['Birth Date'].str[0:4]
Chemistry['Birth Date'] = Chemistry['Birth Date'].replace(to_replace="nan", value=0)
Chemistry['Birth Date'] = Chemistry['Birth Date'].apply(pd.to_numeric)
Chemistry["Age"] = Chemistry["Year"] - Chemistry["Birth Date"]
```


```python
bins = [29, 39, 49, 59, 69, 79, 89,]
groupNames = ['30s', '40s', '50s', '60s', '70s', '80s']
Chemistry['Age Categorical'] = pd.cut(Chemistry['Age'], bins, labels=groupNames)

```


```python
ChemistryAge = Chemistry['Age Categorical'].value_counts()
print(ChemistryAge)

sns.barplot(x=ChemistryAge.index,y=ChemistryAge.values)
plt.xticks(rotation=90)
plt.title('Age differences')
plt.show()
```

    50s    57
    60s    46
    40s    34
    70s    22
    80s     7
    30s     7
    Name: Age Categorical, dtype: int64
    


![png](DataVisualization_files/DataVisualization_31_1.png)


## Age differences between the Economics category


```python
Economics['Birth Date'] = Economics['Birth Date'].str[0:4]
Economics['Birth Date'] = Economics['Birth Date'].replace(to_replace="nan", value=0)
Economics['Birth Date'] = Economics['Birth Date'].apply(pd.to_numeric)
Economics["Age"] = Economics["Year"] - Economics["Birth Date"]
```


```python
bins = [49, 59, 69, 79, 89, 100]
groupNames = ['50s', '60s', '70s', '80s', '90s']
Economics['Age Categorical'] = pd.cut(Economics['Age'], bins, labels=groupNames)
```


```python
EconomicsAge = Economics['Age Categorical'].value_counts()
print(EconomicsAge)

sns.barplot(x=EconomicsAge.index,y=EconomicsAge.values)
plt.xticks(rotation=90)
plt.title('Age differences')
plt.show()
```

    60s    39
    70s    22
    50s    12
    80s     4
    90s     1
    Name: Age Categorical, dtype: int64
    


![png](DataVisualization_files/DataVisualization_35_1.png)


## Age differences between the Literature  category


```python
Literature['Birth Date'] = Literature['Birth Date'].str[0:4]
Literature['Birth Date'] = Literature['Birth Date'].replace(to_replace="nan", value=0)
Literature['Birth Date'] = Literature['Birth Date'].apply(pd.to_numeric)
Literature["Age"] = Literature["Year"] - Literature["Birth Date"]
```


```python
bins = [39, 49, 59, 69, 79, 89]
groupNames = ['40s', '50s', '60s', '70s', '80s']
Literature['Age Categorical'] = pd.cut(Literature['Age'], bins, labels=groupNames)
```


```python
LiteratureAge = Literature['Age Categorical'].value_counts()
print(LiteratureAge)

sns.barplot(x=LiteratureAge.index,y=LiteratureAge.values)
plt.xticks(rotation=90)
plt.title('Age differences')
plt.show()
```

    60s    37
    70s    33
    50s    28
    40s     9
    80s     6
    Name: Age Categorical, dtype: int64
    


![png](DataVisualization_files/DataVisualization_39_1.png)


## Age differences between the Medicine category


```python
Medicine['Birth Date'] = Medicine['Birth Date'].str[0:4]
Medicine['Birth Date'] = Medicine['Birth Date'].replace(to_replace="nan", value=0)
Medicine['Birth Date'] = Medicine['Birth Date'].apply(pd.to_numeric)
Medicine["Age"] = Medicine["Year"] - Medicine["Birth Date"]
```


```python
bins = [29, 39, 49, 59, 69, 79, 89]
groupNames = ['30s', '40s', '50s', '60s', '70s', '80s']
Medicine['Age Categorical'] = pd.cut(Medicine['Age'], bins, labels=groupNames)
```


```python
MedicineAge = Medicine['Age Categorical'].value_counts()
print(MedicineAge)

sns.barplot(x=MedicineAge.index,y=MedicineAge.values)
plt.xticks(rotation=90)
plt.title('Age differences')
plt.show()
```

    50s    63
    60s    54
    40s    43
    70s    29
    30s    12
    80s    10
    Name: Age Categorical, dtype: int64
    


![png](DataVisualization_files/DataVisualization_43_1.png)


## Age differences between the Peace category


```python
Peace['Birth Date'] = Peace['Birth Date'].str[0:4]
Peace['Birth Date'] = Peace['Birth Date'].replace(to_replace="nan", value=0)
Peace['Birth Date'] = Peace['Birth Date'].apply(pd.to_numeric)
Peace["Age"] = Peace["Year"] - Peace["Birth Date"]
```


```python
bins = [0, 29, 39, 49, 59, 69, 79, 89]
groupNames = ['Teens','30s', '40s', '50s', '60s', '70s', '80s']
Peace['Age Categorical'] = pd.cut(Peace['Age'], bins, labels=groupNames)
```


```python
PeaceAge = Peace['Age Categorical'].value_counts()
print(PeaceAge)

sns.barplot(x=PeaceAge.index,y=PeaceAge.values)
plt.xticks(rotation=90)
plt.title('Age differences')
plt.show()
```

    60s      31
    70s      26
    50s      19
    40s      14
    80s       6
    30s       6
    Teens     1
    Name: Age Categorical, dtype: int64
    


![png](DataVisualization_files/DataVisualization_47_1.png)


## Age differences between the Physics category


```python
Physics['Birth Date'] = Physics['Birth Date'].str[0:4]
Physics['Birth Date'] = Physics['Birth Date'].replace(to_replace="nan", value=0)
Physics['Birth Date'] = Physics['Birth Date'].apply(pd.to_numeric)
Physics["Age"] = Physics["Year"] - Physics["Birth Date"]
```


```python
bins = [19, 29, 39, 49, 59, 69, 79, 89]
groupNames = ['20s', '30s', '40s', '50s', '60s', '70s', '80s']
Physics['Age Categorical'] = pd.cut(Physics['Age'], bins, labels=groupNames)
```


```python
PhysicsAge = Physics['Age Categorical'].value_counts()
print(PhysicsAge)

sns.barplot(x=PhysicsAge.index,y=PhysicsAge.values)
plt.xticks(rotation=90)
plt.title('Age differences')
plt.show()
```

    40s    55
    50s    49
    60s    41
    70s    23
    30s    23
    80s    11
    20s     1
    Name: Age Categorical, dtype: int64
    


![png](DataVisualization_files/DataVisualization_51_1.png)


## Distribution of Age of Winners in each Category using boxplot


```python
sns.boxplot(data=NobelData,
         x='Category',
         y='Age')

plt.show()
```


![png](DataVisualization_files/DataVisualization_53_0.png)


## Organizations with the largest number of nobel prizes


```python
org = NobelData['Organization Name'].value_counts().reset_index().head(20)

sns.barplot(x='Organization Name',y='index',data=org)
plt.xticks(rotation=90)
plt.ylabel('Organization Name')
plt.xlabel('Count')
plt.show()
```


![png](DataVisualization_files/DataVisualization_55_0.png)


## Oldest nobel prize winners


```python
OldestWinners=NobelData.nlargest(5,'Age')
display(OldestWinners[['Category','Full Name','Birth Country','Sex','Age']])
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
      <th>Category</th>
      <th>Full Name</th>
      <th>Birth Country</th>
      <th>Sex</th>
      <th>Age</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>767</th>
      <td>Economics</td>
      <td>Leonid Hurwicz</td>
      <td>Russia</td>
      <td>Male</td>
      <td>90</td>
    </tr>
    <tr>
      <th>829</th>
      <td>Economics</td>
      <td>Lloyd S. Shapley</td>
      <td>United States of America</td>
      <td>Male</td>
      <td>89</td>
    </tr>
    <tr>
      <th>720</th>
      <td>Physics</td>
      <td>Raymond Davis Jr.</td>
      <td>United States of America</td>
      <td>Male</td>
      <td>88</td>
    </tr>
    <tr>
      <th>770</th>
      <td>Literature</td>
      <td>Doris Lessing</td>
      <td>Persia (Iran)</td>
      <td>Female</td>
      <td>88</td>
    </tr>
    <tr>
      <th>360</th>
      <td>Medicine</td>
      <td>Peyton Rous</td>
      <td>United States of America</td>
      <td>Male</td>
      <td>87</td>
    </tr>
  </tbody>
</table>
</div>


## Youngest nobel prize winners


```python
YoungestWinner=NobelData.nsmallest(5,'Age')
display(YoungestWinner[['Category','Full Name','Birth Country','Sex','Age']])
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
      <th>Category</th>
      <th>Full Name</th>
      <th>Birth Country</th>
      <th>Sex</th>
      <th>Age</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>856</th>
      <td>Peace</td>
      <td>Malala Yousafzai</td>
      <td>Pakistan</td>
      <td>Female</td>
      <td>17</td>
    </tr>
    <tr>
      <th>82</th>
      <td>Physics</td>
      <td>William Lawrence Bragg</td>
      <td>Australia</td>
      <td>Male</td>
      <td>25</td>
    </tr>
    <tr>
      <th>162</th>
      <td>Physics</td>
      <td>Werner Karl Heisenberg</td>
      <td>Germany</td>
      <td>Male</td>
      <td>31</td>
    </tr>
    <tr>
      <th>167</th>
      <td>Physics</td>
      <td>Paul Adrien Maurice Dirac</td>
      <td>United Kingdom</td>
      <td>Male</td>
      <td>31</td>
    </tr>
    <tr>
      <th>185</th>
      <td>Physics</td>
      <td>Carl David Anderson</td>
      <td>United States of America</td>
      <td>Male</td>
      <td>31</td>
    </tr>
  </tbody>
</table>
</div>


## First woman to win the nobel prize


```python
female=NobelData[NobelData['Sex']=="Female"].nsmallest(1,'Year')
female[['Year','Category','Full Name','Prize']]
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
      <th>Year</th>
      <th>Category</th>
      <th>Full Name</th>
      <th>Prize</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>19</th>
      <td>1903</td>
      <td>Physics</td>
      <td>Marie Curie, née Sklodowska</td>
      <td>The Nobel Prize in Physics 1903</td>
    </tr>
  </tbody>
</table>
</div>



## Repeat laureates


```python
repeat=NobelData.groupby(['Category','Full Name']).filter(lambda group : len(group)>=2)
repeat
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
      <th>Unnamed: 0</th>
      <th>Year</th>
      <th>Category</th>
      <th>Prize</th>
      <th>Prize Share</th>
      <th>Laureate ID</th>
      <th>Laureate Type</th>
      <th>Full Name</th>
      <th>Birth Date</th>
      <th>Birth City</th>
      <th>Birth Country</th>
      <th>Sex</th>
      <th>Organization Name</th>
      <th>Organization City</th>
      <th>Organization Country</th>
      <th>Age</th>
      <th>Age Categorical</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>45</th>
      <td>46</td>
      <td>1908</td>
      <td>Medicine</td>
      <td>The Nobel Prize in Physiology or Medicine 1908</td>
      <td>1/2</td>
      <td>302</td>
      <td>Individual</td>
      <td>Paul Ehrlich</td>
      <td>1854</td>
      <td>Strehlen (Strzelin)</td>
      <td>Prussia (Poland)</td>
      <td>Male</td>
      <td>Goettingen University</td>
      <td>Göttingen</td>
      <td>Germany</td>
      <td>54</td>
      <td>50s</td>
    </tr>
    <tr>
      <th>46</th>
      <td>47</td>
      <td>1908</td>
      <td>Medicine</td>
      <td>The Nobel Prize in Physiology or Medicine 1908</td>
      <td>1/2</td>
      <td>302</td>
      <td>Individual</td>
      <td>Paul Ehrlich</td>
      <td>1854</td>
      <td>Strehlen (Strzelin)</td>
      <td>Prussia (Poland)</td>
      <td>Male</td>
      <td>Königliches Institut für experimentelle Therap...</td>
      <td>Frankfurt-on-the-Main</td>
      <td>Germany</td>
      <td>54</td>
      <td>50s</td>
    </tr>
    <tr>
      <th>154</th>
      <td>157</td>
      <td>1931</td>
      <td>Chemistry</td>
      <td>The Nobel Prize in Chemistry 1931</td>
      <td>1/2</td>
      <td>189</td>
      <td>Individual</td>
      <td>Carl Bosch</td>
      <td>1874</td>
      <td>Cologne</td>
      <td>Germany</td>
      <td>Male</td>
      <td>University of Heidelberg</td>
      <td>Heidelberg</td>
      <td>Germany</td>
      <td>57</td>
      <td>50s</td>
    </tr>
    <tr>
      <th>155</th>
      <td>158</td>
      <td>1931</td>
      <td>Chemistry</td>
      <td>The Nobel Prize in Chemistry 1931</td>
      <td>1/2</td>
      <td>189</td>
      <td>Individual</td>
      <td>Carl Bosch</td>
      <td>1874</td>
      <td>Cologne</td>
      <td>Germany</td>
      <td>Male</td>
      <td>I.G. Farbenindustrie A.G.</td>
      <td>Heidelberg</td>
      <td>Germany</td>
      <td>57</td>
      <td>50s</td>
    </tr>
    <tr>
      <th>156</th>
      <td>159</td>
      <td>1931</td>
      <td>Chemistry</td>
      <td>The Nobel Prize in Chemistry 1931</td>
      <td>1/2</td>
      <td>190</td>
      <td>Individual</td>
      <td>Friedrich Bergius</td>
      <td>1884</td>
      <td>Goldschmieden, near Breslau</td>
      <td>Germany (Poland)</td>
      <td>Male</td>
      <td>University of Heidelberg</td>
      <td>Heidelberg</td>
      <td>Germany</td>
      <td>47</td>
      <td>40s</td>
    </tr>
    <tr>
      <th>157</th>
      <td>160</td>
      <td>1931</td>
      <td>Chemistry</td>
      <td>The Nobel Prize in Chemistry 1931</td>
      <td>1/2</td>
      <td>190</td>
      <td>Individual</td>
      <td>Friedrich Bergius</td>
      <td>1884</td>
      <td>Goldschmieden, near Breslau</td>
      <td>Germany (Poland)</td>
      <td>Male</td>
      <td>I.G. Farbenindustrie A.G.</td>
      <td>Mannheim-Rheinau</td>
      <td>Germany</td>
      <td>47</td>
      <td>40s</td>
    </tr>
    <tr>
      <th>176</th>
      <td>179</td>
      <td>1934</td>
      <td>Medicine</td>
      <td>The Nobel Prize in Physiology or Medicine 1934</td>
      <td>1/3</td>
      <td>328</td>
      <td>Individual</td>
      <td>William Parry Murphy</td>
      <td>1892</td>
      <td>Stoughton, WI</td>
      <td>United States of America</td>
      <td>Male</td>
      <td>Harvard University</td>
      <td>Cambridge, MA</td>
      <td>United States of America</td>
      <td>42</td>
      <td>40s</td>
    </tr>
    <tr>
      <th>177</th>
      <td>180</td>
      <td>1934</td>
      <td>Medicine</td>
      <td>The Nobel Prize in Physiology or Medicine 1934</td>
      <td>1/3</td>
      <td>328</td>
      <td>Individual</td>
      <td>William Parry Murphy</td>
      <td>1892</td>
      <td>Stoughton, WI</td>
      <td>United States of America</td>
      <td>Male</td>
      <td>Peter Brent Brigham Hospital</td>
      <td>Boston, MA</td>
      <td>United States of America</td>
      <td>42</td>
      <td>40s</td>
    </tr>
    <tr>
      <th>184</th>
      <td>187</td>
      <td>1936</td>
      <td>Chemistry</td>
      <td>The Nobel Prize in Chemistry 1936</td>
      <td>1/1</td>
      <td>195</td>
      <td>Individual</td>
      <td>Petrus (Peter) Josephus Wilhelmus Debye</td>
      <td>1884</td>
      <td>Maastricht</td>
      <td>Netherlands</td>
      <td>Male</td>
      <td>Berlin University</td>
      <td>Berlin</td>
      <td>Germany</td>
      <td>52</td>
      <td>50s</td>
    </tr>
    <tr>
      <th>185</th>
      <td>188</td>
      <td>1936</td>
      <td>Chemistry</td>
      <td>The Nobel Prize in Chemistry 1936</td>
      <td>1/1</td>
      <td>195</td>
      <td>Individual</td>
      <td>Petrus (Peter) Josephus Wilhelmus Debye</td>
      <td>1884</td>
      <td>Maastricht</td>
      <td>Netherlands</td>
      <td>Male</td>
      <td>Kaiser-Wilhelm-Institut (now Max-Planck-Instit...</td>
      <td>Berlin</td>
      <td>Germany</td>
      <td>52</td>
      <td>50s</td>
    </tr>
    <tr>
      <th>199</th>
      <td>202</td>
      <td>1938</td>
      <td>Chemistry</td>
      <td>The Nobel Prize in Chemistry 1938</td>
      <td>1/1</td>
      <td>198</td>
      <td>Individual</td>
      <td>Richard Kuhn</td>
      <td>1900</td>
      <td>Vienna</td>
      <td>Austria-Hungary (Austria)</td>
      <td>Male</td>
      <td>Kaiser-Wilhelm-Institut (now Max-Planck Instit...</td>
      <td>Heidelberg</td>
      <td>Germany</td>
      <td>38</td>
      <td>30s</td>
    </tr>
    <tr>
      <th>200</th>
      <td>203</td>
      <td>1938</td>
      <td>Chemistry</td>
      <td>The Nobel Prize in Chemistry 1938</td>
      <td>1/1</td>
      <td>198</td>
      <td>Individual</td>
      <td>Richard Kuhn</td>
      <td>1900</td>
      <td>Vienna</td>
      <td>Austria-Hungary (Austria)</td>
      <td>Male</td>
      <td>University of Heidelberg</td>
      <td>Heidelberg</td>
      <td>Germany</td>
      <td>38</td>
      <td>30s</td>
    </tr>
    <tr>
      <th>204</th>
      <td>208</td>
      <td>1939</td>
      <td>Chemistry</td>
      <td>The Nobel Prize in Chemistry 1939</td>
      <td>1/2</td>
      <td>199</td>
      <td>Individual</td>
      <td>Adolf Friedrich Johann Butenandt</td>
      <td>1903</td>
      <td>Bremerhaven-Lehe</td>
      <td>Germany</td>
      <td>Male</td>
      <td>Kaiser-Wilhelm-Institut (now Max-Planck-Instit...</td>
      <td>Berlin-Dahlem</td>
      <td>Germany</td>
      <td>36</td>
      <td>30s</td>
    </tr>
    <tr>
      <th>205</th>
      <td>209</td>
      <td>1939</td>
      <td>Chemistry</td>
      <td>The Nobel Prize in Chemistry 1939</td>
      <td>1/2</td>
      <td>199</td>
      <td>Individual</td>
      <td>Adolf Friedrich Johann Butenandt</td>
      <td>1903</td>
      <td>Bremerhaven-Lehe</td>
      <td>Germany</td>
      <td>Male</td>
      <td>Berlin University</td>
      <td>Berlin</td>
      <td>Germany</td>
      <td>36</td>
      <td>30s</td>
    </tr>
    <tr>
      <th>247</th>
      <td>254</td>
      <td>1949</td>
      <td>Medicine</td>
      <td>The Nobel Prize in Physiology or Medicine 1949</td>
      <td>1/2</td>
      <td>348</td>
      <td>Individual</td>
      <td>Antonio Caetano de Abreu Freire Egas Moniz</td>
      <td>1874</td>
      <td>Avanca</td>
      <td>Portugal</td>
      <td>Male</td>
      <td>University of Lisbon</td>
      <td>Lisbon</td>
      <td>Portugal</td>
      <td>75</td>
      <td>70s</td>
    </tr>
    <tr>
      <th>248</th>
      <td>255</td>
      <td>1949</td>
      <td>Medicine</td>
      <td>The Nobel Prize in Physiology or Medicine 1949</td>
      <td>1/2</td>
      <td>348</td>
      <td>Individual</td>
      <td>Antonio Caetano de Abreu Freire Egas Moniz</td>
      <td>1874</td>
      <td>Avanca</td>
      <td>Portugal</td>
      <td>Male</td>
      <td>Neurological Institute</td>
      <td>Lisbon</td>
      <td>Portugal</td>
      <td>75</td>
      <td>70s</td>
    </tr>
    <tr>
      <th>250</th>
      <td>257</td>
      <td>1949</td>
      <td>Physics</td>
      <td>The Nobel Prize in Physics 1949</td>
      <td>1/1</td>
      <td>54</td>
      <td>Individual</td>
      <td>Hideki Yukawa</td>
      <td>1907</td>
      <td>Tokyo</td>
      <td>Japan</td>
      <td>Male</td>
      <td>Kyoto Imperial University</td>
      <td>Kyoto</td>
      <td>Japan</td>
      <td>42</td>
      <td>40s</td>
    </tr>
    <tr>
      <th>251</th>
      <td>258</td>
      <td>1949</td>
      <td>Physics</td>
      <td>The Nobel Prize in Physics 1949</td>
      <td>1/1</td>
      <td>54</td>
      <td>Individual</td>
      <td>Hideki Yukawa</td>
      <td>1907</td>
      <td>Tokyo</td>
      <td>Japan</td>
      <td>Male</td>
      <td>Columbia University</td>
      <td>New York, NY</td>
      <td>United States of America</td>
      <td>42</td>
      <td>40s</td>
    </tr>
    <tr>
      <th>274</th>
      <td>281</td>
      <td>1953</td>
      <td>Chemistry</td>
      <td>The Nobel Prize in Chemistry 1953</td>
      <td>1/1</td>
      <td>216</td>
      <td>Individual</td>
      <td>Hermann Staudinger</td>
      <td>1881</td>
      <td>Worms</td>
      <td>Germany</td>
      <td>Male</td>
      <td>University of Freiburg</td>
      <td>Breisgau</td>
      <td>Federal Republic of Germany</td>
      <td>72</td>
      <td>70s</td>
    </tr>
    <tr>
      <th>275</th>
      <td>282</td>
      <td>1953</td>
      <td>Chemistry</td>
      <td>The Nobel Prize in Chemistry 1953</td>
      <td>1/1</td>
      <td>216</td>
      <td>Individual</td>
      <td>Hermann Staudinger</td>
      <td>1881</td>
      <td>Worms</td>
      <td>Germany</td>
      <td>Male</td>
      <td>Staatliches Institut für makromolekulare Chemi...</td>
      <td>Breisgau</td>
      <td>Federal Republic of Germany</td>
      <td>72</td>
      <td>70s</td>
    </tr>
    <tr>
      <th>278</th>
      <td>285</td>
      <td>1953</td>
      <td>Medicine</td>
      <td>The Nobel Prize in Physiology or Medicine 1953</td>
      <td>1/2</td>
      <td>355</td>
      <td>Individual</td>
      <td>Fritz Albert Lipmann</td>
      <td>1899</td>
      <td>Koenigsberg (Kaliningrad)</td>
      <td>Germany (Russia)</td>
      <td>Male</td>
      <td>Harvard Medical School</td>
      <td>Boston, MA</td>
      <td>United States of America</td>
      <td>54</td>
      <td>50s</td>
    </tr>
    <tr>
      <th>279</th>
      <td>286</td>
      <td>1953</td>
      <td>Medicine</td>
      <td>The Nobel Prize in Physiology or Medicine 1953</td>
      <td>1/2</td>
      <td>355</td>
      <td>Individual</td>
      <td>Fritz Albert Lipmann</td>
      <td>1899</td>
      <td>Koenigsberg (Kaliningrad)</td>
      <td>Germany (Russia)</td>
      <td>Male</td>
      <td>Massachusetts General Hospital</td>
      <td>Boston, MA</td>
      <td>United States of America</td>
      <td>54</td>
      <td>50s</td>
    </tr>
    <tr>
      <th>284</th>
      <td>291</td>
      <td>1954</td>
      <td>Medicine</td>
      <td>The Nobel Prize in Physiology or Medicine 1954</td>
      <td>1/3</td>
      <td>356</td>
      <td>Individual</td>
      <td>John Franklin Enders</td>
      <td>1897</td>
      <td>West Hartford, CT</td>
      <td>United States of America</td>
      <td>Male</td>
      <td>Harvard Medical School</td>
      <td>Boston, MA</td>
      <td>United States of America</td>
      <td>57</td>
      <td>50s</td>
    </tr>
    <tr>
      <th>285</th>
      <td>292</td>
      <td>1954</td>
      <td>Medicine</td>
      <td>The Nobel Prize in Physiology or Medicine 1954</td>
      <td>1/3</td>
      <td>356</td>
      <td>Individual</td>
      <td>John Franklin Enders</td>
      <td>1897</td>
      <td>West Hartford, CT</td>
      <td>United States of America</td>
      <td>Male</td>
      <td>Research Division of Infectious Diseases, Chil...</td>
      <td>Boston, MA</td>
      <td>United States of America</td>
      <td>57</td>
      <td>50s</td>
    </tr>
    <tr>
      <th>289</th>
      <td>297</td>
      <td>1954</td>
      <td>Physics</td>
      <td>The Nobel Prize in Physics 1954</td>
      <td>1/2</td>
      <td>62</td>
      <td>Individual</td>
      <td>Walther Bothe</td>
      <td>1891</td>
      <td>Oranienburg</td>
      <td>Germany</td>
      <td>Male</td>
      <td>University of Heidelberg</td>
      <td>Heidelberg</td>
      <td>Federal Republic of Germany</td>
      <td>63</td>
      <td>60s</td>
    </tr>
    <tr>
      <th>290</th>
      <td>298</td>
      <td>1954</td>
      <td>Physics</td>
      <td>The Nobel Prize in Physics 1954</td>
      <td>1/2</td>
      <td>62</td>
      <td>Individual</td>
      <td>Walther Bothe</td>
      <td>1891</td>
      <td>Oranienburg</td>
      <td>Germany</td>
      <td>Male</td>
      <td>Max-Planck-Institut für medizinische Forschung</td>
      <td>Heidelberg</td>
      <td>Federal Republic of Germany</td>
      <td>63</td>
      <td>60s</td>
    </tr>
    <tr>
      <th>300</th>
      <td>308</td>
      <td>1956</td>
      <td>Medicine</td>
      <td>The Nobel Prize in Physiology or Medicine 1956</td>
      <td>1/3</td>
      <td>361</td>
      <td>Individual</td>
      <td>Werner Forssmann</td>
      <td>1904</td>
      <td>Berlin</td>
      <td>Germany</td>
      <td>Male</td>
      <td>Mainz University</td>
      <td>Mainz</td>
      <td>Federal Republic of Germany</td>
      <td>52</td>
      <td>50s</td>
    </tr>
    <tr>
      <th>301</th>
      <td>309</td>
      <td>1956</td>
      <td>Medicine</td>
      <td>The Nobel Prize in Physiology or Medicine 1956</td>
      <td>1/3</td>
      <td>361</td>
      <td>Individual</td>
      <td>Werner Forssmann</td>
      <td>1904</td>
      <td>Berlin</td>
      <td>Germany</td>
      <td>Male</td>
      <td>Self</td>
      <td>Bad Kreuznach</td>
      <td>Federal Republic of Germany</td>
      <td>52</td>
      <td>50s</td>
    </tr>
    <tr>
      <th>304</th>
      <td>312</td>
      <td>1956</td>
      <td>Physics</td>
      <td>The Nobel Prize in Physics 1956</td>
      <td>1/3</td>
      <td>66</td>
      <td>Individual</td>
      <td>John Bardeen</td>
      <td>1908</td>
      <td>Madison, WI</td>
      <td>United States of America</td>
      <td>Male</td>
      <td>University of Illinois</td>
      <td>Urbana, IL</td>
      <td>United States of America</td>
      <td>48</td>
      <td>40s</td>
    </tr>
    <tr>
      <th>312</th>
      <td>320</td>
      <td>1958</td>
      <td>Chemistry</td>
      <td>The Nobel Prize in Chemistry 1958</td>
      <td>1/1</td>
      <td>222</td>
      <td>Individual</td>
      <td>Frederick Sanger</td>
      <td>1918</td>
      <td>Rendcombe</td>
      <td>United Kingdom</td>
      <td>Male</td>
      <td>University of Cambridge</td>
      <td>Cambridge</td>
      <td>United Kingdom</td>
      <td>40</td>
      <td>40s</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>864</th>
      <td>887</td>
      <td>2011</td>
      <td>Medicine</td>
      <td>The Nobel Prize in Physiology or Medicine 2011</td>
      <td>1/4</td>
      <td>861</td>
      <td>Individual</td>
      <td>Bruce A. Beutler</td>
      <td>1957</td>
      <td>Chicago, IL</td>
      <td>United States of America</td>
      <td>Male</td>
      <td>University of Texas Southwestern Medical Cente...</td>
      <td>Dallas, TX</td>
      <td>United States of America</td>
      <td>54</td>
      <td>50s</td>
    </tr>
    <tr>
      <th>865</th>
      <td>888</td>
      <td>2011</td>
      <td>Medicine</td>
      <td>The Nobel Prize in Physiology or Medicine 2011</td>
      <td>1/4</td>
      <td>861</td>
      <td>Individual</td>
      <td>Bruce A. Beutler</td>
      <td>1957</td>
      <td>Chicago, IL</td>
      <td>United States of America</td>
      <td>Male</td>
      <td>The Scripps Research Institute</td>
      <td>La Jolla, CA</td>
      <td>United States of America</td>
      <td>54</td>
      <td>50s</td>
    </tr>
    <tr>
      <th>871</th>
      <td>894</td>
      <td>2011</td>
      <td>Physics</td>
      <td>The Nobel Prize in Physics 2011</td>
      <td>1/2</td>
      <td>864</td>
      <td>Individual</td>
      <td>Saul Perlmutter</td>
      <td>1959</td>
      <td>Champaign-Urbana, IL</td>
      <td>United States of America</td>
      <td>Male</td>
      <td>Lawrence Berkeley National Laboratory</td>
      <td>Berkeley, CA</td>
      <td>United States of America</td>
      <td>52</td>
      <td>50s</td>
    </tr>
    <tr>
      <th>872</th>
      <td>895</td>
      <td>2011</td>
      <td>Physics</td>
      <td>The Nobel Prize in Physics 2011</td>
      <td>1/2</td>
      <td>864</td>
      <td>Individual</td>
      <td>Saul Perlmutter</td>
      <td>1959</td>
      <td>Champaign-Urbana, IL</td>
      <td>United States of America</td>
      <td>Male</td>
      <td>University of California</td>
      <td>Berkeley, CA</td>
      <td>United States of America</td>
      <td>52</td>
      <td>50s</td>
    </tr>
    <tr>
      <th>874</th>
      <td>897</td>
      <td>2011</td>
      <td>Physics</td>
      <td>The Nobel Prize in Physics 2011</td>
      <td>1/4</td>
      <td>866</td>
      <td>Individual</td>
      <td>Adam G. Riess</td>
      <td>1969</td>
      <td>Washington, DC</td>
      <td>United States of America</td>
      <td>Male</td>
      <td>Johns Hopkins University</td>
      <td>Baltimore, MD</td>
      <td>United States of America</td>
      <td>42</td>
      <td>40s</td>
    </tr>
    <tr>
      <th>875</th>
      <td>898</td>
      <td>2011</td>
      <td>Physics</td>
      <td>The Nobel Prize in Physics 2011</td>
      <td>1/4</td>
      <td>866</td>
      <td>Individual</td>
      <td>Adam G. Riess</td>
      <td>1969</td>
      <td>Washington, DC</td>
      <td>United States of America</td>
      <td>Male</td>
      <td>Space Telescope Science Institute</td>
      <td>Baltimore, MD</td>
      <td>United States of America</td>
      <td>42</td>
      <td>40s</td>
    </tr>
    <tr>
      <th>876</th>
      <td>899</td>
      <td>2012</td>
      <td>Chemistry</td>
      <td>The Nobel Prize in Chemistry 2012</td>
      <td>1/2</td>
      <td>878</td>
      <td>Individual</td>
      <td>Robert J. Lefkowitz</td>
      <td>1943</td>
      <td>New York, NY</td>
      <td>United States of America</td>
      <td>Male</td>
      <td>Howard Hughes Medical Institute</td>
      <td>Maryland</td>
      <td>USA</td>
      <td>69</td>
      <td>60s</td>
    </tr>
    <tr>
      <th>877</th>
      <td>900</td>
      <td>2012</td>
      <td>Chemistry</td>
      <td>The Nobel Prize in Chemistry 2012</td>
      <td>1/2</td>
      <td>878</td>
      <td>Individual</td>
      <td>Robert J. Lefkowitz</td>
      <td>1943</td>
      <td>New York, NY</td>
      <td>United States of America</td>
      <td>Male</td>
      <td>Duke University Medical Center</td>
      <td>Maryland</td>
      <td>USA</td>
      <td>69</td>
      <td>60s</td>
    </tr>
    <tr>
      <th>879</th>
      <td>902</td>
      <td>2012</td>
      <td>Economics</td>
      <td>The Sveriges Riksbank Prize in Economic Scienc...</td>
      <td>1/2</td>
      <td>882</td>
      <td>Individual</td>
      <td>Alvin E. Roth</td>
      <td>1951</td>
      <td>New York, NY</td>
      <td>United States of America</td>
      <td>Male</td>
      <td>Harvard University</td>
      <td>Cambridge, MA</td>
      <td>United States of America</td>
      <td>61</td>
      <td>60s</td>
    </tr>
    <tr>
      <th>880</th>
      <td>903</td>
      <td>2012</td>
      <td>Economics</td>
      <td>The Sveriges Riksbank Prize in Economic Scienc...</td>
      <td>1/2</td>
      <td>882</td>
      <td>Individual</td>
      <td>Alvin E. Roth</td>
      <td>1951</td>
      <td>New York, NY</td>
      <td>United States of America</td>
      <td>Male</td>
      <td>Harvard Business School</td>
      <td>Boston, MA</td>
      <td>United States of America</td>
      <td>61</td>
      <td>60s</td>
    </tr>
    <tr>
      <th>884</th>
      <td>907</td>
      <td>2012</td>
      <td>Medicine</td>
      <td>The Nobel Prize in Physiology or Medicine 2012</td>
      <td>1/2</td>
      <td>875</td>
      <td>Individual</td>
      <td>Shinya Yamanaka</td>
      <td>1962</td>
      <td>Osaka</td>
      <td>Japan</td>
      <td>Male</td>
      <td>Kyoto University</td>
      <td>Kyoto</td>
      <td>Japan</td>
      <td>50</td>
      <td>50s</td>
    </tr>
    <tr>
      <th>885</th>
      <td>908</td>
      <td>2012</td>
      <td>Medicine</td>
      <td>The Nobel Prize in Physiology or Medicine 2012</td>
      <td>1/2</td>
      <td>875</td>
      <td>Individual</td>
      <td>Shinya Yamanaka</td>
      <td>1962</td>
      <td>Osaka</td>
      <td>Japan</td>
      <td>Male</td>
      <td>Gladstone Institutes</td>
      <td>San Francisco, CA</td>
      <td>United States of America</td>
      <td>50</td>
      <td>50s</td>
    </tr>
    <tr>
      <th>886</th>
      <td>910</td>
      <td>2012</td>
      <td>Physics</td>
      <td>The Nobel Prize in Physics 2012</td>
      <td>1/2</td>
      <td>876</td>
      <td>Individual</td>
      <td>Serge Haroche</td>
      <td>1944</td>
      <td>Casablanca</td>
      <td>Morocco</td>
      <td>Male</td>
      <td>Collège de France</td>
      <td>Paris</td>
      <td>France</td>
      <td>68</td>
      <td>60s</td>
    </tr>
    <tr>
      <th>887</th>
      <td>911</td>
      <td>2012</td>
      <td>Physics</td>
      <td>The Nobel Prize in Physics 2012</td>
      <td>1/2</td>
      <td>876</td>
      <td>Individual</td>
      <td>Serge Haroche</td>
      <td>1944</td>
      <td>Casablanca</td>
      <td>Morocco</td>
      <td>Male</td>
      <td>École Normale Supérieure</td>
      <td>Paris</td>
      <td>France</td>
      <td>68</td>
      <td>60s</td>
    </tr>
    <tr>
      <th>888</th>
      <td>912</td>
      <td>2012</td>
      <td>Physics</td>
      <td>The Nobel Prize in Physics 2012</td>
      <td>1/2</td>
      <td>877</td>
      <td>Individual</td>
      <td>David J. Wineland</td>
      <td>1944</td>
      <td>Milwaukee, WI</td>
      <td>United States of America</td>
      <td>Male</td>
      <td>National Institute of Standards and Technology</td>
      <td>Boulder, CO</td>
      <td>United States of America</td>
      <td>68</td>
      <td>60s</td>
    </tr>
    <tr>
      <th>889</th>
      <td>913</td>
      <td>2012</td>
      <td>Physics</td>
      <td>The Nobel Prize in Physics 2012</td>
      <td>1/2</td>
      <td>877</td>
      <td>Individual</td>
      <td>David J. Wineland</td>
      <td>1944</td>
      <td>Milwaukee, WI</td>
      <td>United States of America</td>
      <td>Male</td>
      <td>University of Colorado</td>
      <td>Boulder, CO</td>
      <td>United States of America</td>
      <td>68</td>
      <td>60s</td>
    </tr>
    <tr>
      <th>890</th>
      <td>914</td>
      <td>2013</td>
      <td>Chemistry</td>
      <td>The Nobel Prize in Chemistry 2013</td>
      <td>1/3</td>
      <td>889</td>
      <td>Individual</td>
      <td>Martin Karplus</td>
      <td>1930</td>
      <td>Vienna</td>
      <td>Austria</td>
      <td>Male</td>
      <td>Université de Strasbourg</td>
      <td>Strasbourg</td>
      <td>France</td>
      <td>83</td>
      <td>80s</td>
    </tr>
    <tr>
      <th>891</th>
      <td>915</td>
      <td>2013</td>
      <td>Chemistry</td>
      <td>The Nobel Prize in Chemistry 2013</td>
      <td>1/3</td>
      <td>889</td>
      <td>Individual</td>
      <td>Martin Karplus</td>
      <td>1930</td>
      <td>Vienna</td>
      <td>Austria</td>
      <td>Male</td>
      <td>Harvard University</td>
      <td>Cambridge, MA</td>
      <td>United States of America</td>
      <td>83</td>
      <td>80s</td>
    </tr>
    <tr>
      <th>899</th>
      <td>923</td>
      <td>2013</td>
      <td>Medicine</td>
      <td>The Nobel Prize in Physiology or Medicine 2013</td>
      <td>1/3</td>
      <td>885</td>
      <td>Individual</td>
      <td>Randy W. Schekman</td>
      <td>1948</td>
      <td>St. Paul, MN</td>
      <td>United States of America</td>
      <td>Male</td>
      <td>University of California</td>
      <td>Maryland</td>
      <td>USA</td>
      <td>65</td>
      <td>60s</td>
    </tr>
    <tr>
      <th>900</th>
      <td>924</td>
      <td>2013</td>
      <td>Medicine</td>
      <td>The Nobel Prize in Physiology or Medicine 2013</td>
      <td>1/3</td>
      <td>885</td>
      <td>Individual</td>
      <td>Randy W. Schekman</td>
      <td>1948</td>
      <td>St. Paul, MN</td>
      <td>United States of America</td>
      <td>Male</td>
      <td>Howard Hughes Medical Institute</td>
      <td>Maryland</td>
      <td>USA</td>
      <td>65</td>
      <td>60s</td>
    </tr>
    <tr>
      <th>901</th>
      <td>925</td>
      <td>2013</td>
      <td>Medicine</td>
      <td>The Nobel Prize in Physiology or Medicine 2013</td>
      <td>1/3</td>
      <td>886</td>
      <td>Individual</td>
      <td>Thomas C. Südhof</td>
      <td>1955</td>
      <td>Göttingen</td>
      <td>Germany</td>
      <td>Male</td>
      <td>Stanford University</td>
      <td>Maryland</td>
      <td>USA</td>
      <td>58</td>
      <td>50s</td>
    </tr>
    <tr>
      <th>902</th>
      <td>926</td>
      <td>2013</td>
      <td>Medicine</td>
      <td>The Nobel Prize in Physiology or Medicine 2013</td>
      <td>1/3</td>
      <td>886</td>
      <td>Individual</td>
      <td>Thomas C. Südhof</td>
      <td>1955</td>
      <td>Göttingen</td>
      <td>Germany</td>
      <td>Male</td>
      <td>Howard Hughes Medical Institute</td>
      <td>Maryland</td>
      <td>USA</td>
      <td>58</td>
      <td>50s</td>
    </tr>
    <tr>
      <th>906</th>
      <td>931</td>
      <td>2014</td>
      <td>Chemistry</td>
      <td>The Nobel Prize in Chemistry 2014</td>
      <td>1/3</td>
      <td>910</td>
      <td>Individual</td>
      <td>Stefan W. Hell</td>
      <td>1962</td>
      <td>Arad</td>
      <td>Romania</td>
      <td>Male</td>
      <td>Max Planck Institute for Biophysical Chemistry</td>
      <td>Göttingen</td>
      <td>Germany</td>
      <td>52</td>
      <td>50s</td>
    </tr>
    <tr>
      <th>907</th>
      <td>932</td>
      <td>2014</td>
      <td>Chemistry</td>
      <td>The Nobel Prize in Chemistry 2014</td>
      <td>1/3</td>
      <td>910</td>
      <td>Individual</td>
      <td>Stefan W. Hell</td>
      <td>1962</td>
      <td>Arad</td>
      <td>Romania</td>
      <td>Male</td>
      <td>German Cancer Research Center</td>
      <td>Heidelberg</td>
      <td>Germany</td>
      <td>52</td>
      <td>50s</td>
    </tr>
    <tr>
      <th>916</th>
      <td>941</td>
      <td>2014</td>
      <td>Physics</td>
      <td>The Nobel Prize in Physics 2014</td>
      <td>1/3</td>
      <td>906</td>
      <td>Individual</td>
      <td>Isamu Akasaki</td>
      <td>1929</td>
      <td>Chiran</td>
      <td>Japan</td>
      <td>Male</td>
      <td>Meijo University</td>
      <td>Nagoya</td>
      <td>Japan</td>
      <td>85</td>
      <td>80s</td>
    </tr>
    <tr>
      <th>917</th>
      <td>942</td>
      <td>2014</td>
      <td>Physics</td>
      <td>The Nobel Prize in Physics 2014</td>
      <td>1/3</td>
      <td>906</td>
      <td>Individual</td>
      <td>Isamu Akasaki</td>
      <td>1929</td>
      <td>Chiran</td>
      <td>Japan</td>
      <td>Male</td>
      <td>Nagoya University</td>
      <td>Nagoya</td>
      <td>Japan</td>
      <td>85</td>
      <td>80s</td>
    </tr>
    <tr>
      <th>920</th>
      <td>945</td>
      <td>2015</td>
      <td>Chemistry</td>
      <td>The Nobel Prize in Chemistry 2015</td>
      <td>1/3</td>
      <td>921</td>
      <td>Individual</td>
      <td>Tomas Lindahl</td>
      <td>1938</td>
      <td>Stockholm</td>
      <td>Sweden</td>
      <td>Male</td>
      <td>Francis Crick Institute</td>
      <td>Hertfordshire</td>
      <td>United Kingdom</td>
      <td>77</td>
      <td>70s</td>
    </tr>
    <tr>
      <th>921</th>
      <td>946</td>
      <td>2015</td>
      <td>Chemistry</td>
      <td>The Nobel Prize in Chemistry 2015</td>
      <td>1/3</td>
      <td>921</td>
      <td>Individual</td>
      <td>Tomas Lindahl</td>
      <td>1938</td>
      <td>Stockholm</td>
      <td>Sweden</td>
      <td>Male</td>
      <td>Clare Hall Laboratory</td>
      <td>Hertfordshire</td>
      <td>United Kingdom</td>
      <td>77</td>
      <td>70s</td>
    </tr>
    <tr>
      <th>922</th>
      <td>947</td>
      <td>2015</td>
      <td>Chemistry</td>
      <td>The Nobel Prize in Chemistry 2015</td>
      <td>1/3</td>
      <td>922</td>
      <td>Individual</td>
      <td>Paul Modrich</td>
      <td>1946</td>
      <td>Raton, NM</td>
      <td>United States of America</td>
      <td>Male</td>
      <td>Howard Hughes Medical Institute</td>
      <td>Durham, NC</td>
      <td>United States of America</td>
      <td>69</td>
      <td>60s</td>
    </tr>
    <tr>
      <th>923</th>
      <td>948</td>
      <td>2015</td>
      <td>Chemistry</td>
      <td>The Nobel Prize in Chemistry 2015</td>
      <td>1/3</td>
      <td>922</td>
      <td>Individual</td>
      <td>Paul Modrich</td>
      <td>1946</td>
      <td>Raton, NM</td>
      <td>United States of America</td>
      <td>Male</td>
      <td>Duke University School of Medicine</td>
      <td>Durham, NC</td>
      <td>United States of America</td>
      <td>69</td>
      <td>60s</td>
    </tr>
  </tbody>
</table>
<p>119 rows × 17 columns</p>
</div>



## How old are you when you get the prize?


```python
sns.lmplot('Year','Age',data=NobelData,lowess=True, aspect=2,  line_kws={'color' : 'black'})
plt.show()
```


![png](DataVisualization_files/DataVisualization_65_0.png)



```python

```
