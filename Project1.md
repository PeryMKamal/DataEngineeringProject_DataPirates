# Overview
###### The Nobel Prize is perhaps the world's most well known scientific award. Every year it is given to scientists and scholars in chemistry, literature, physics, medicine, economics, and peace. The first Nobel Prize was handed out in 1901, and at that time the prize was Eurocentric and male-focused, but nowadays it's not biased in any way. Surely, right? Well, we’re going to find out! The Nobel Foundation has made a dataset available of all prize winners from the start of the prize, in 1901, to 2016. Let’s load it in and check it out.

## Imports


```python
import numpy as np
import os
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
```

## Reading Data


```python

dff = pd.read_csv('archive.csv')
df = dff.copy()
df.sample(5)
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
      <th>Prize</th>
      <th>Motivation</th>
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
      <th>Death Date</th>
      <th>Death City</th>
      <th>Death Country</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>446</th>
      <td>1973</td>
      <td>Economics</td>
      <td>The Sveriges Riksbank Prize in Economic Scienc...</td>
      <td>"for the development of the input-output metho...</td>
      <td>1/1</td>
      <td>683</td>
      <td>Individual</td>
      <td>Wassily Leontief</td>
      <td>1906-08-05</td>
      <td>St. Petersburg</td>
      <td>Russia</td>
      <td>Male</td>
      <td>Harvard University</td>
      <td>Cambridge, MA</td>
      <td>United States of America</td>
      <td>1999-02-05</td>
      <td>New York, NY</td>
      <td>United States of America</td>
    </tr>
    <tr>
      <th>278</th>
      <td>1952</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 1952</td>
      <td>NaN</td>
      <td>1/1</td>
      <td>513</td>
      <td>Individual</td>
      <td>Albert Schweitzer</td>
      <td>1875-01-14</td>
      <td>Kaysersberg</td>
      <td>Germany (France)</td>
      <td>Male</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1965-09-04</td>
      <td>Lambaréné</td>
      <td>Gabon</td>
    </tr>
    <tr>
      <th>606</th>
      <td>1988</td>
      <td>Literature</td>
      <td>The Nobel Prize in Literature 1988</td>
      <td>"who, through works rich in nuance - now clear...</td>
      <td>1/1</td>
      <td>665</td>
      <td>Individual</td>
      <td>Naguib Mahfouz</td>
      <td>1911-12-11</td>
      <td>Cairo</td>
      <td>Egypt</td>
      <td>Male</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2006-08-30</td>
      <td>Cairo</td>
      <td>Egypt</td>
    </tr>
    <tr>
      <th>675</th>
      <td>1995</td>
      <td>Chemistry</td>
      <td>The Nobel Prize in Chemistry 1995</td>
      <td>"for their work in atmospheric chemistry, part...</td>
      <td>1/3</td>
      <td>283</td>
      <td>Individual</td>
      <td>F. Sherwood Rowland</td>
      <td>1927-06-28</td>
      <td>Delaware, OH</td>
      <td>United States of America</td>
      <td>Male</td>
      <td>University of California</td>
      <td>Irvine, CA</td>
      <td>United States of America</td>
      <td>2012-03-10</td>
      <td>Corona del Mar, CA</td>
      <td>United States of America</td>
    </tr>
    <tr>
      <th>782</th>
      <td>2003</td>
      <td>Physics</td>
      <td>The Nobel Prize in Physics 2003</td>
      <td>"for pioneering contributions to the theory of...</td>
      <td>1/3</td>
      <td>766</td>
      <td>Individual</td>
      <td>Alexei A. Abrikosov</td>
      <td>1928-06-25</td>
      <td>Moscow</td>
      <td>Union of Soviet Socialist Republics (Russia)</td>
      <td>Male</td>
      <td>Argonne National Laboratory</td>
      <td>Argonne, IL</td>
      <td>United States of America</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



## Data Exploration and Cleaning

### How many nobel prizes we have between 1901 and 2016


```python
print("Shape\n%s Rows \n%s Columns " % (df.shape[0],df.shape[1]))
```

    Shape
    969 Rows 
    18 Columns 
    

### How many male nobel prizes and how many female nobel prizes


```python
df1 = df.groupby(['Sex']).size()
print (df1)
```

    Sex
    Female     50
    Male      893
    dtype: int64
    

## Different nationalities winning the nobel prize


```python
df2 =  df.groupby("Birth Country").agg('size')

print(df2.sort_values( ascending=False))

```

    Birth Country
    United States of America                  276
    United Kingdom                             88
    Germany                                    70
    France                                     53
    Sweden                                     30
                                             ... 
    Ottoman Empire (Republic of Macedonia)      1
    Ottoman Empire (Turkey)                     1
    Pakistan                                    1
    Persia (Iran)                               1
    Java, Dutch East Indies (Indonesia)         1
    Length: 121, dtype: int64
    

### From the previous group by we can conclude that the USA dominates in the noble prizes birth country as there is 276 nobel prize holder from USA

### Calculate the proption of female lauretes per decade


```python
df3=df[df["Sex"] == "Female"].groupby("Year").size()
print(df3)
```

    Year
    1903    1
    1905    1
    1909    1
    1911    1
    1926    1
    1928    1
    1931    1
    1935    1
    1938    1
    1945    1
    1946    1
    1947    1
    1963    1
    1964    1
    1966    1
    1976    2
    1977    1
    1979    1
    1982    1
    1983    1
    1986    1
    1988    1
    1991    2
    1992    1
    1993    1
    1995    1
    1996    1
    1997    1
    2003    1
    2004    3
    2007    1
    2008    1
    2009    6
    2011    3
    2013    1
    2014    2
    2015    2
    dtype: int64
    

### Calculate the proption of female lauretes per category


```python
df4=df[df["Sex"] == "Female"].groupby("Category").size()
print(df4)
```

    Category
    Chemistry      4
    Economics      2
    Literature    14
    Medicine      12
    Peace         16
    Physics        2
    dtype: int64
    

### The first woman to win the Nobel Prize



```python
s=df[df["Sex"] == "Female"]
s2=s.sort_values(by=['Year'])
s3=s2.nsmallest(1,'Year')
s3
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
      <th>Prize</th>
      <th>Motivation</th>
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
      <th>Death Date</th>
      <th>Death City</th>
      <th>Death Country</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>19</th>
      <td>1903</td>
      <td>Physics</td>
      <td>The Nobel Prize in Physics 1903</td>
      <td>"in recognition of the extraordinary services ...</td>
      <td>1/4</td>
      <td>6</td>
      <td>Individual</td>
      <td>Marie Curie, née Sklodowska</td>
      <td>1867-11-07</td>
      <td>Warsaw</td>
      <td>Russian Empire (Poland)</td>
      <td>Female</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1934-07-04</td>
      <td>Sallanches</td>
      <td>France</td>
    </tr>
  </tbody>
</table>
</div>



### Who won the nobel prize more than once?


```python
df5=  df.groupby("Full Name").agg('size')
print(df5.sort_values( ascending=False))

```

    Full Name
    Jack W. Szostak                                                                      3
    Comité international de la Croix Rouge (International Committee of the Red Cross)    3
    Randy W. Schekman                                                                    2
    Robert J. Lefkowitz                                                                  2
    Roderick MacKinnon                                                                   2
                                                                                        ..
    National Dialogue Quartet                                                            1
    Naguib Mahfouz                                                                       1
    Nadine Gordimer                                                                      1
    Médecins Sans Frontières                                                             1
    A. Michael Spence                                                                    1
    Length: 904, dtype: int64
    

### To Know the number of entries and the columns


```python
df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 969 entries, 0 to 968
    Data columns (total 18 columns):
    Year                    969 non-null int64
    Category                969 non-null object
    Prize                   969 non-null object
    Motivation              881 non-null object
    Prize Share             969 non-null object
    Laureate ID             969 non-null int64
    Laureate Type           969 non-null object
    Full Name               969 non-null object
    Birth Date              940 non-null object
    Birth City              941 non-null object
    Birth Country           943 non-null object
    Sex                     943 non-null object
    Organization Name       722 non-null object
    Organization City       716 non-null object
    Organization Country    716 non-null object
    Death Date              617 non-null object
    Death City              599 non-null object
    Death Country           605 non-null object
    dtypes: int64(2), object(16)
    memory usage: 136.4+ KB
    

### Checking how many null values for each column


```python
df.isnull().sum()
```




    Year                      0
    Category                  0
    Prize                     0
    Motivation               88
    Prize Share               0
    Laureate ID               0
    Laureate Type             0
    Full Name                 0
    Birth Date               29
    Birth City               28
    Birth Country            26
    Sex                      26
    Organization Name       247
    Organization City       253
    Organization Country    253
    Death Date              352
    Death City              370
    Death Country           364
    dtype: int64



### What are the values for the Laureate Type


```python
pd.unique(df["Laureate Type"])
```




    array(['Individual', 'Organization'], dtype=object)



### Number of Cells having Laureate type = Organization


```python
df[df["Laureate Type"] == "Organization"].shape
```




    (30, 18)




```python
df[df["Sex"].isnull()]
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
      <th>Prize</th>
      <th>Motivation</th>
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
      <th>Death Date</th>
      <th>Death City</th>
      <th>Death Country</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>24</th>
      <td>1904</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 1904</td>
      <td>NaN</td>
      <td>1/1</td>
      <td>467</td>
      <td>Organization</td>
      <td>Institut de droit international (Institute of ...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>61</th>
      <td>1910</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 1910</td>
      <td>NaN</td>
      <td>1/1</td>
      <td>477</td>
      <td>Organization</td>
      <td>Bureau international permanent de la Paix (Per...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>90</th>
      <td>1917</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 1917</td>
      <td>NaN</td>
      <td>1/1</td>
      <td>482</td>
      <td>Organization</td>
      <td>Comité international de la Croix Rouge (Intern...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>206</th>
      <td>1938</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 1938</td>
      <td>NaN</td>
      <td>1/1</td>
      <td>503</td>
      <td>Organization</td>
      <td>Office international Nansen pour les Réfugiés ...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>222</th>
      <td>1944</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 1944</td>
      <td>NaN</td>
      <td>1/1</td>
      <td>482</td>
      <td>Organization</td>
      <td>Comité international de la Croix Rouge (Intern...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>244</th>
      <td>1947</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 1947</td>
      <td>NaN</td>
      <td>1/2</td>
      <td>508</td>
      <td>Organization</td>
      <td>Friends Service Council (The Quakers)</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>245</th>
      <td>1947</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 1947</td>
      <td>NaN</td>
      <td>1/2</td>
      <td>509</td>
      <td>Organization</td>
      <td>American Friends Service Committee (The Quakers)</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>295</th>
      <td>1954</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 1954</td>
      <td>NaN</td>
      <td>1/1</td>
      <td>515</td>
      <td>Organization</td>
      <td>Office of the United Nations High Commissioner...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>365</th>
      <td>1963</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 1963</td>
      <td>NaN</td>
      <td>1/2</td>
      <td>482</td>
      <td>Organization</td>
      <td>Comité international de la Croix Rouge (Intern...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>366</th>
      <td>1963</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 1963</td>
      <td>NaN</td>
      <td>1/2</td>
      <td>523</td>
      <td>Organization</td>
      <td>Ligue des Sociétés de la Croix-Rouge (League o...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>383</th>
      <td>1965</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 1965</td>
      <td>NaN</td>
      <td>1/1</td>
      <td>525</td>
      <td>Organization</td>
      <td>United Nations Children's Fund (UNICEF)</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>416</th>
      <td>1969</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 1969</td>
      <td>NaN</td>
      <td>1/1</td>
      <td>527</td>
      <td>Organization</td>
      <td>International Labour Organization (I.L.O.)</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>498</th>
      <td>1977</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 1977</td>
      <td>NaN</td>
      <td>1/1</td>
      <td>537</td>
      <td>Organization</td>
      <td>Amnesty International</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>543</th>
      <td>1981</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 1981</td>
      <td>NaN</td>
      <td>1/1</td>
      <td>515</td>
      <td>Organization</td>
      <td>Office of the United Nations High Commissioner...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>578</th>
      <td>1985</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 1985</td>
      <td>NaN</td>
      <td>1/1</td>
      <td>547</td>
      <td>Organization</td>
      <td>International Physicians for the Prevention of...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>610</th>
      <td>1988</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 1988</td>
      <td>NaN</td>
      <td>1/1</td>
      <td>550</td>
      <td>Organization</td>
      <td>United Nations Peacekeeping Forces</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>682</th>
      <td>1995</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 1995</td>
      <td>"for their efforts to diminish the part played...</td>
      <td>1/2</td>
      <td>561</td>
      <td>Organization</td>
      <td>Pugwash Conferences on Science and World Affairs</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>705</th>
      <td>1997</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 1997</td>
      <td>"for their work for the banning and clearing o...</td>
      <td>1/2</td>
      <td>564</td>
      <td>Organization</td>
      <td>International Campaign to Ban Landmines (ICBL)</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>728</th>
      <td>1999</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 1999</td>
      <td>"in recognition of the organization's pioneeri...</td>
      <td>1/1</td>
      <td>568</td>
      <td>Organization</td>
      <td>Médecins Sans Frontières</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>754</th>
      <td>2001</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 2001</td>
      <td>"for their work for a better organized and mor...</td>
      <td>1/2</td>
      <td>748</td>
      <td>Organization</td>
      <td>United Nations (U.N.)</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>808</th>
      <td>2005</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 2005</td>
      <td>"for their efforts to prevent nuclear energy f...</td>
      <td>1/2</td>
      <td>797</td>
      <td>Organization</td>
      <td>International Atomic Energy Agency (IAEA)</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>821</th>
      <td>2006</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 2006</td>
      <td>"for their efforts to create economic and soci...</td>
      <td>1/2</td>
      <td>810</td>
      <td>Organization</td>
      <td>Grameen Bank</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>833</th>
      <td>2007</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 2007</td>
      <td>"for their efforts to build up and disseminate...</td>
      <td>1/2</td>
      <td>818</td>
      <td>Organization</td>
      <td>Intergovernmental Panel on Climate Change (IPCC)</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>909</th>
      <td>2012</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 2012</td>
      <td>"for over six decades contributed to the advan...</td>
      <td>1/1</td>
      <td>881</td>
      <td>Organization</td>
      <td>European Union (EU)</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>927</th>
      <td>2013</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 2013</td>
      <td>"for its extensive efforts to eliminate chemic...</td>
      <td>1/1</td>
      <td>893</td>
      <td>Organization</td>
      <td>Organisation for the Prohibition of Chemical W...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>955</th>
      <td>2015</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 2015</td>
      <td>"for its decisive contribution to the building...</td>
      <td>1/1</td>
      <td>925</td>
      <td>Organization</td>
      <td>National Dialogue Quartet</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```python
#df[df["Prize Share"] == "1/2"]
df[df["Full Name"] == "Paul Ehrlich"]
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
      <th>Prize</th>
      <th>Motivation</th>
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
      <th>Death Date</th>
      <th>Death City</th>
      <th>Death Country</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>46</th>
      <td>1908</td>
      <td>Medicine</td>
      <td>The Nobel Prize in Physiology or Medicine 1908</td>
      <td>"in recognition of their work on immunity"</td>
      <td>1/2</td>
      <td>302</td>
      <td>Individual</td>
      <td>Paul Ehrlich</td>
      <td>1854-03-14</td>
      <td>Strehlen (Strzelin)</td>
      <td>Prussia (Poland)</td>
      <td>Male</td>
      <td>Goettingen University</td>
      <td>Göttingen</td>
      <td>Germany</td>
      <td>1915-08-20</td>
      <td>Bad Homburg vor der Höhe</td>
      <td>Germany</td>
    </tr>
    <tr>
      <th>47</th>
      <td>1908</td>
      <td>Medicine</td>
      <td>The Nobel Prize in Physiology or Medicine 1908</td>
      <td>"in recognition of their work on immunity"</td>
      <td>1/2</td>
      <td>302</td>
      <td>Individual</td>
      <td>Paul Ehrlich</td>
      <td>1854-03-14</td>
      <td>Strehlen (Strzelin)</td>
      <td>Prussia (Poland)</td>
      <td>Male</td>
      <td>Königliches Institut für experimentelle Therap...</td>
      <td>Frankfurt-on-the-Main</td>
      <td>Germany</td>
      <td>1915-08-20</td>
      <td>Bad Homburg vor der Höhe</td>
      <td>Germany</td>
    </tr>
  </tbody>
</table>
</div>




```python

#df[df["Organization Name"].isnull()]
l = df[df["Organization Name"].isnull()]
k = l[l["Laureate Type"] == "Organization"]
print("Shape\n%s Rows \n%s Columns " % (k.shape[0],k.shape[1]))

```

    Shape
    30 Rows 
    18 Columns 
    

## Cleaning 

### Removing Duplicates 


```python
print(len(df))
df.drop_duplicates(subset = "Laureate ID",keep = "first", inplace = True)
print(len(df))
df[df["Full Name"] == "Paul Ehrlich"]
```

    969
    904
    




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
      <th>Prize</th>
      <th>Motivation</th>
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
      <th>Death Date</th>
      <th>Death City</th>
      <th>Death Country</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>46</th>
      <td>1908</td>
      <td>Medicine</td>
      <td>The Nobel Prize in Physiology or Medicine 1908</td>
      <td>"in recognition of their work on immunity"</td>
      <td>1/2</td>
      <td>302</td>
      <td>Individual</td>
      <td>Paul Ehrlich</td>
      <td>1854-03-14</td>
      <td>Strehlen (Strzelin)</td>
      <td>Prussia (Poland)</td>
      <td>Male</td>
      <td>Goettingen University</td>
      <td>Göttingen</td>
      <td>Germany</td>
      <td>1915-08-20</td>
      <td>Bad Homburg vor der Höhe</td>
      <td>Germany</td>
    </tr>
  </tbody>
</table>
</div>



### Deleting Motivation values


```python
del df['Motivation']
df.sample(5)
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
      <th>Death Date</th>
      <th>Death City</th>
      <th>Death Country</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>572</th>
      <td>1985</td>
      <td>Chemistry</td>
      <td>The Nobel Prize in Chemistry 1985</td>
      <td>1/2</td>
      <td>262</td>
      <td>Individual</td>
      <td>Herbert A. Hauptman</td>
      <td>1917-02-14</td>
      <td>New York, NY</td>
      <td>United States of America</td>
      <td>Male</td>
      <td>The Medical Foundation of Buffalo</td>
      <td>Buffalo, NY</td>
      <td>United States of America</td>
      <td>2011-10-23</td>
      <td>Buffalo, NY</td>
      <td>United States of America</td>
    </tr>
    <tr>
      <th>432</th>
      <td>1971</td>
      <td>Physics</td>
      <td>The Nobel Prize in Physics 1971</td>
      <td>1/1</td>
      <td>93</td>
      <td>Individual</td>
      <td>Dennis Gabor</td>
      <td>1900-06-05</td>
      <td>Budapest</td>
      <td>Hungary</td>
      <td>Male</td>
      <td>Imperial College</td>
      <td>London</td>
      <td>United Kingdom</td>
      <td>1979-02-08</td>
      <td>London</td>
      <td>United Kingdom</td>
    </tr>
    <tr>
      <th>238</th>
      <td>1946</td>
      <td>Physics</td>
      <td>The Nobel Prize in Physics 1946</td>
      <td>1/1</td>
      <td>51</td>
      <td>Individual</td>
      <td>Percy Williams Bridgman</td>
      <td>1882-04-21</td>
      <td>Cambridge, MA</td>
      <td>United States of America</td>
      <td>Male</td>
      <td>Harvard University</td>
      <td>Cambridge, MA</td>
      <td>United States of America</td>
      <td>1961-08-20</td>
      <td>Randolph, NH</td>
      <td>United States of America</td>
    </tr>
    <tr>
      <th>545</th>
      <td>1981</td>
      <td>Physics</td>
      <td>The Nobel Prize in Physics 1981</td>
      <td>1/4</td>
      <td>119</td>
      <td>Individual</td>
      <td>Arthur Leonard Schawlow</td>
      <td>1921-05-05</td>
      <td>Mount Verno, NY</td>
      <td>United States of America</td>
      <td>Male</td>
      <td>Stanford University</td>
      <td>Stanford, CA</td>
      <td>United States of America</td>
      <td>1999-04-28</td>
      <td>Palo Alto, CA</td>
      <td>United States of America</td>
    </tr>
    <tr>
      <th>126</th>
      <td>1925</td>
      <td>Physics</td>
      <td>The Nobel Prize in Physics 1925</td>
      <td>1/2</td>
      <td>30</td>
      <td>Individual</td>
      <td>James Franck</td>
      <td>1882-08-26</td>
      <td>Hamburg</td>
      <td>Germany</td>
      <td>Male</td>
      <td>Goettingen University</td>
      <td>Göttingen</td>
      <td>Germany</td>
      <td>1964-05-21</td>
      <td>Göttingen</td>
      <td>West Germany (Germany)</td>
    </tr>
  </tbody>
</table>
</div>



### Fixing Incorrect Data of Laureate Type ( Org -> Individual )


```python

#df.loc[df["Sex"].isnull(), "Sex"] = "Org"

mask = (df["Laureate Type"] == "Organization") & (df["Birth Date"].notnull())
df["Laureate Type"][mask] = "Individual"

l = df[df["Laureate Type"] == "Organization"]
k = l[l["Birth Date"].notnull()]
k
```

    C:\Users\Administrator\Anaconda3\lib\site-packages\ipykernel_launcher.py:4: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      after removing the cwd from sys.path.
    




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
      <th>Death Date</th>
      <th>Death City</th>
      <th>Death Country</th>
    </tr>
  </thead>
  <tbody>
  </tbody>
</table>
</div>




```python
k = df[df["Laureate Type"] == "Organization"]
print("Shape\n%s Rows \n%s Columns " % (k.shape[0],k.shape[1]))
k
#df.isnull().sum()
```

    Shape
    23 Rows 
    17 Columns 
    




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
      <th>Death Date</th>
      <th>Death City</th>
      <th>Death Country</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>24</th>
      <td>1904</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 1904</td>
      <td>1/1</td>
      <td>467</td>
      <td>Organization</td>
      <td>Institut de droit international (Institute of ...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>61</th>
      <td>1910</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 1910</td>
      <td>1/1</td>
      <td>477</td>
      <td>Organization</td>
      <td>Bureau international permanent de la Paix (Per...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>90</th>
      <td>1917</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 1917</td>
      <td>1/1</td>
      <td>482</td>
      <td>Organization</td>
      <td>Comité international de la Croix Rouge (Intern...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>206</th>
      <td>1938</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 1938</td>
      <td>1/1</td>
      <td>503</td>
      <td>Organization</td>
      <td>Office international Nansen pour les Réfugiés ...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>244</th>
      <td>1947</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 1947</td>
      <td>1/2</td>
      <td>508</td>
      <td>Organization</td>
      <td>Friends Service Council (The Quakers)</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>245</th>
      <td>1947</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 1947</td>
      <td>1/2</td>
      <td>509</td>
      <td>Organization</td>
      <td>American Friends Service Committee (The Quakers)</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>295</th>
      <td>1954</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 1954</td>
      <td>1/1</td>
      <td>515</td>
      <td>Organization</td>
      <td>Office of the United Nations High Commissioner...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>366</th>
      <td>1963</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 1963</td>
      <td>1/2</td>
      <td>523</td>
      <td>Organization</td>
      <td>Ligue des Sociétés de la Croix-Rouge (League o...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>383</th>
      <td>1965</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 1965</td>
      <td>1/1</td>
      <td>525</td>
      <td>Organization</td>
      <td>United Nations Children's Fund (UNICEF)</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>416</th>
      <td>1969</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 1969</td>
      <td>1/1</td>
      <td>527</td>
      <td>Organization</td>
      <td>International Labour Organization (I.L.O.)</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>498</th>
      <td>1977</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 1977</td>
      <td>1/1</td>
      <td>537</td>
      <td>Organization</td>
      <td>Amnesty International</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>578</th>
      <td>1985</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 1985</td>
      <td>1/1</td>
      <td>547</td>
      <td>Organization</td>
      <td>International Physicians for the Prevention of...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>610</th>
      <td>1988</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 1988</td>
      <td>1/1</td>
      <td>550</td>
      <td>Organization</td>
      <td>United Nations Peacekeeping Forces</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>682</th>
      <td>1995</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 1995</td>
      <td>1/2</td>
      <td>561</td>
      <td>Organization</td>
      <td>Pugwash Conferences on Science and World Affairs</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>705</th>
      <td>1997</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 1997</td>
      <td>1/2</td>
      <td>564</td>
      <td>Organization</td>
      <td>International Campaign to Ban Landmines (ICBL)</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>728</th>
      <td>1999</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 1999</td>
      <td>1/1</td>
      <td>568</td>
      <td>Organization</td>
      <td>Médecins Sans Frontières</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>754</th>
      <td>2001</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 2001</td>
      <td>1/2</td>
      <td>748</td>
      <td>Organization</td>
      <td>United Nations (U.N.)</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>808</th>
      <td>2005</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 2005</td>
      <td>1/2</td>
      <td>797</td>
      <td>Organization</td>
      <td>International Atomic Energy Agency (IAEA)</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>821</th>
      <td>2006</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 2006</td>
      <td>1/2</td>
      <td>810</td>
      <td>Organization</td>
      <td>Grameen Bank</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>833</th>
      <td>2007</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 2007</td>
      <td>1/2</td>
      <td>818</td>
      <td>Organization</td>
      <td>Intergovernmental Panel on Climate Change (IPCC)</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>909</th>
      <td>2012</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 2012</td>
      <td>1/1</td>
      <td>881</td>
      <td>Organization</td>
      <td>European Union (EU)</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>927</th>
      <td>2013</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 2013</td>
      <td>1/1</td>
      <td>893</td>
      <td>Organization</td>
      <td>Organisation for the Prohibition of Chemical W...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>955</th>
      <td>2015</td>
      <td>Peace</td>
      <td>The Nobel Peace Prize 2015</td>
      <td>1/1</td>
      <td>925</td>
      <td>Organization</td>
      <td>National Dialogue Quartet</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



### Dropping organization since they only represent 2.5% of the dataframe ( 23 rows out of 904 rows)


```python
print("Shape\n%s Rows \n%s Columns " % (df.shape[0],df.shape[1]))
df = df[df["Laureate Type"] == "Individual"]
print("Shape\n%s Rows \n%s Columns " % (df.shape[0],df.shape[1]))
df[df["Laureate Type"] == "Individual"]
df.isnull().sum()
#df[df["Death Date"].isnull()]
```

    Shape
    904 Rows 
    17 Columns 
    Shape
    881 Rows 
    17 Columns 
    




    Year                      0
    Category                  0
    Prize                     0
    Prize Share               0
    Laureate ID               0
    Laureate Type             0
    Full Name                 0
    Birth Date                2
    Birth City                2
    Birth Country             0
    Sex                       0
    Organization Name       220
    Organization City       218
    Organization Country    218
    Death Date              292
    Death City              309
    Death Country           303
    dtype: int64



### Dropping the Death City, Death Date, Death Country as they will not affect Data visualisation in any way


```python
df = df.drop("Death Date", axis=1)
df = df.drop("Death City", axis=1)
df = df.drop("Death Country", axis=1)
df.isnull().sum()

```




    Year                      0
    Category                  0
    Prize                     0
    Prize Share               0
    Laureate ID               0
    Laureate Type             0
    Full Name                 0
    Birth Date                2
    Birth City                2
    Birth Country             0
    Sex                       0
    Organization Name       220
    Organization City       218
    Organization Country    218
    dtype: int64




### Adding missing birth date and city for individuals 


```python
df.loc[df["Laureate ID"] == 841, "Birth Date"] = "1952-04-01" #venk
df.loc[df["Laureate ID"] == 864, "Birth Date"] = "1959-09-22" #saul
df.loc[df["Laureate ID"] == 747, "Birth City"] = "Chaguanas" # sir Vidiadhar
df.loc[df["Laureate ID"] == 855, "Birth City"] = "Changchun" # liu Xiaobo
df.isnull().sum()
```




    Year                      0
    Category                  0
    Prize                     0
    Prize Share               0
    Laureate ID               0
    Laureate Type             0
    Full Name                 0
    Birth Date                0
    Birth City                0
    Birth Country             0
    Sex                       0
    Organization Name       220
    Organization City       218
    Organization Country    218
    dtype: int64




```python

df.loc[df["Laureate ID"] == 6, "Organization Country"] = "self" 
df.loc[df["Laureate ID"] == 6, "Organization City"] = "self" 
df.loc[df["Laureate ID"] == 6, "Organization Name"] = "self" 

df.loc[df["Laureate ID"] == 318, "Organization Country"] = "Tunisia" 

df.loc[df["Laureate ID"] == 684, "Organization Country"] = "self" 
df.loc[df["Laureate ID"] == 684, "Organization City"] = "self" 
df.loc[df["Laureate ID"] == 684, "Organization Name"] = "self" 

df.loc[df["Laureate ID"] == 685, "Organization Country"] = "self" 
df.loc[df["Laureate ID"] == 685, "Organization City"] = "self" 
df.loc[df["Laureate ID"] == 685, "Organization Name"] = "self" 

df.loc[df["Laureate ID"] == 270, "Organization Country"] = "USA" 
df.loc[df["Laureate ID"] == 270, "Organization City"] = "Maryland" 

df.loc[df["Laureate ID"] == 461, "Organization Country"] = "USA" 
df.loc[df["Laureate ID"] == 461, "Organization City"] = "Maryland" 

df.loc[df["Laureate ID"] == 770, "Organization Country"] = "USA" 
df.loc[df["Laureate ID"] == 770, "Organization City"] = "Maryland"

df.loc[df["Laureate ID"] == 811, "Organization Country"] = "USA" 
df.loc[df["Laureate ID"] == 811, "Organization City"] = "Maryland"

df.loc[df["Laureate ID"] == 831, "Organization Country"] = "USA" 
df.loc[df["Laureate ID"] == 831, "Organization City"] = "Maryland" 

df.loc[df["Laureate ID"] == 842, "Organization Country"] = "USA" 
df.loc[df["Laureate ID"] == 842, "Organization City"] = "Maryland" 

df.loc[df["Laureate ID"] == 837, "Organization Country"] = "USA" 
df.loc[df["Laureate ID"] == 837, "Organization City"] = "Maryland" 

df.loc[df["Laureate ID"] == 878, "Organization Country"] = "USA" 
df.loc[df["Laureate ID"] == 878, "Organization City"] = "Maryland" 

df.loc[df["Laureate ID"] == 885, "Organization Country"] = "USA" 
df.loc[df["Laureate ID"] == 885, "Organization City"] = "Maryland" 

df.loc[df["Laureate ID"] == 886, "Organization Country"] = "USA" 
df.loc[df["Laureate ID"] == 886, "Organization City"] = "Maryland" 

k = df[df["Organization Country"].isnull()]
l = k[k["Category"] != "Peace"]
l[l["Category"] != "Literature"]

df[df["Organization Country"].isnull()].shape


# ba3d keda el hytba22a meen b2a? el peace wl literature bas
# homa el wa7ideen el hyb2o be null, then momken terunno el 3 lines el ta7t
#dool be el text el t7boha, take care el order yb2a keda 3shan law 3mlto 
#3 lines el ta7t dool homa el nas el physics wl mediccine msh ht3rfo tgebeehom tany

df.loc[df["Organization Name"].isnull(), "Organization Name"] = "Self"
df.loc[df["Organization City"].isnull(), "Organization City"] = "Self"
df.loc[df["Organization Country"].isnull(), "Organization Country"] = "Self"

df.isnull().sum()

```




    Year                    0
    Category                0
    Prize                   0
    Prize Share             0
    Laureate ID             0
    Laureate Type           0
    Full Name               0
    Birth Date              0
    Birth City              0
    Birth Country           0
    Sex                     0
    Organization Name       0
    Organization City       0
    Organization Country    0
    dtype: int64




```python
df.to_csv('archiveData_Cleaned.csv')
```


```python

```
