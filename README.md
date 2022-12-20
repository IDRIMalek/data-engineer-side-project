To convert my notebook to a markdown format, I used nbconvert: 

<li>pip install pyppeteer
<li>pip install nbconvert
<li>jupyter nbconvert .\README.ipynb --to markdown

# DATA-ENGINEER-TECHNICAL-TEST Side


Here is the test https://github.com/IDRIMalek/data-engineer-side-project/blob/master/Technical_test_-_Data_Engineer.pdf 

### Read and understand the data you’ve been given


<li> sider_availabilities.csv
<li> task_locations.csv
<li> sider.json
<li> tasks.json

---
## 1-Checking sider_availabilities datas:</br>  
  <ul>
  <li>Data Type
  <li>Count lines
  <li>mode
  <li>duplicates
  <li>availability_type_options normalisation       
  </ul> 


```python
%pip install  pandas
```

    Requirement already satisfied: pandas in c:\users\lzrn3903\appdata\local\programs\python\python310\lib\site-packages (1.5.1)Note: you may need to restart the kernel to use updated packages.
    
    Requirement already satisfied: numpy>=1.21.0 in c:\users\lzrn3903\appdata\local\programs\python\python310\lib\site-packages (from pandas) (1.23.1)
    Requirement already satisfied: python-dateutil>=2.8.1 in c:\users\lzrn3903\appdata\roaming\python\python310\site-packages (from pandas) (2.8.2)
    Requirement already satisfied: pytz>=2020.1 in c:\users\lzrn3903\appdata\local\programs\python\python310\lib\site-packages (from pandas) (2022.6)
    Requirement already satisfied: six>=1.5 in c:\users\lzrn3903\appdata\roaming\python\python310\site-packages (from python-dateutil>=2.8.1->pandas) (1.16.0)
    


```python
import pandas as pd
```


```python
df_sider_availabilities = pd.read_csv('..\data\sider_availabilities.csv',  delimiter=';')
```


```python
df_sider_availabilities.head(10)
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
      <th>sider_id</th>
      <th>availability_type_options</th>
      <th>availability_start_date</th>
      <th>availability_end_date</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>si_1RI4jrlsfeQJnA7Bw6yWrEoj3yA</td>
      <td>{day}</td>
      <td>2022-09-12</td>
      <td>2023-01-12</td>
    </tr>
    <tr>
      <th>1</th>
      <td>si_1qie2LrraKFGNz5Y23e20eBmjtt</td>
      <td>{day,night}</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>si_1a21yC1i91YKWzLccd0SOsrWAI4</td>
      <td>{day}</td>
      <td>2021-05-20</td>
      <td>2021-06-20</td>
    </tr>
    <tr>
      <th>3</th>
      <td>si_1rDjTpA39g97jUxC871PTivDwXf</td>
      <td>{day,night}</td>
      <td>NaN</td>
      <td>2021-12-15</td>
    </tr>
    <tr>
      <th>4</th>
      <td>si_1Savo4DmRDXEHqSLht9DYPAZkil</td>
      <td>{day,night}</td>
      <td>2019-12-21</td>
      <td>2020-04-21</td>
    </tr>
    <tr>
      <th>5</th>
      <td>si_1PxGQ4uuJtkDlb9rKXRUt70Kq8Z</td>
      <td>{day}</td>
      <td>2019-09-26</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>6</th>
      <td>0hw5hBYbTkM4kG1CZ</td>
      <td>{day,night}</td>
      <td>NaN</td>
      <td>2021-10-05</td>
    </tr>
    <tr>
      <th>7</th>
      <td>si_1bJwIVmdU5TlhldHpfVtIo6rlbf</td>
      <td>{day,night}</td>
      <td>NaN</td>
      <td>2021-05-01</td>
    </tr>
    <tr>
      <th>8</th>
      <td>si_1mFirMLmMy8ergAU1LG20hFBxmn</td>
      <td>{day,night}</td>
      <td>2021-01-27</td>
      <td>2022-01-27</td>
    </tr>
    <tr>
      <th>9</th>
      <td>si_1RbbSxGDB8WF3PvYG7VSXb21Jhp</td>
      <td>{day,night}</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Checking modes for availability_type_options
df_sider_availabilities.availability_type_options.unique()

```




    array(['{day}', '{day,night}', '{night}', nan, '{night,day}'],
          dtype=object)




```python
#Normalisation
df_sider_availabilities.availability_type_options=df_sider_availabilities.availability_type_options.replace('{day,night}','full')
df_sider_availabilities.availability_type_options=df_sider_availabilities.availability_type_options.replace('{night,day}','full')
df_sider_availabilities.availability_type_options=df_sider_availabilities.availability_type_options.replace('{day}','day')
df_sider_availabilities.availability_type_options=df_sider_availabilities.availability_type_options.replace('{night}','night')
```


```python
#Checking agin if all has been well Normalised
df_sider_availabilities.availability_type_options.unique()
```




    array(['day', 'full', 'night', nan], dtype=object)




```python
#Checking dublicate 
df_sider_availabilities.duplicated().sum()
```




    0




#### 2-Checking task_locations datas:</br>  
  <ul>
  <li>Data Type, Count lines
  <li>Checking Postal_code format 
  <li>Changing float to postal code    
  </ul> 




```python
df_task_locations = pd.read_csv('..\data\\task_locations.csv',  delimiter=',')
```


```python
#Quick check at the datas
df_task_locations.head(10)
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
      <th>location_id</th>
      <th>city</th>
      <th>postal_code</th>
      <th>country</th>
      <th>address</th>
      <th>confirmed</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0OPfGunr3HYLwRKhE</td>
      <td>Créteil</td>
      <td>94000.0</td>
      <td>FR</td>
      <td>Av. de la France libre, 94000 Créteil, France</td>
      <td>True</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1MqyswOr2XSJhXdBD</td>
      <td>Marseille</td>
      <td>13014.0</td>
      <td>FR</td>
      <td>Av. du Marché national, 13014 Marseille, France</td>
      <td>True</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2M3flfoHbKJjr3Gxq</td>
      <td>Paris</td>
      <td>75018.0</td>
      <td>FR</td>
      <td>189 Rue d'Aubervilliers, 75018 Paris, France</td>
      <td>True</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3EbwFmGD1VJfHxWvI</td>
      <td>Saint-Ouen</td>
      <td>93400.0</td>
      <td>FR</td>
      <td>14 Rue Palouzié, 93400 Saint-Ouen, France</td>
      <td>True</td>
    </tr>
    <tr>
      <th>4</th>
      <td>7AK5TQ8rnwwcKk5so</td>
      <td>Serris</td>
      <td>77700.0</td>
      <td>FR</td>
      <td>14 Cr du Danube, 77700 Serris, France</td>
      <td>True</td>
    </tr>
    <tr>
      <th>5</th>
      <td>8kyDbqM0oujUNpcLn</td>
      <td>Rosny-sous-Bois</td>
      <td>93110.0</td>
      <td>FR</td>
      <td>2 Av. du Général de Gaulle, 93110 Rosny-sous-B...</td>
      <td>True</td>
    </tr>
    <tr>
      <th>6</th>
      <td>DSBHGfx5G1QqJeLi3</td>
      <td>Paris</td>
      <td>75003.0</td>
      <td>FR</td>
      <td>117 Rue Vieille-du-Temple, 75003 Paris, France</td>
      <td>True</td>
    </tr>
    <tr>
      <th>7</th>
      <td>HUdwSdKjucD6U46cc</td>
      <td>Noisy-le-Grand</td>
      <td>93160.0</td>
      <td>FR</td>
      <td>234 Bd du Mont d'Est, 93160 Noisy-le-Grand, Fr...</td>
      <td>True</td>
    </tr>
    <tr>
      <th>8</th>
      <td>HhoegL0m0Emb31LIx</td>
      <td>La Courneuve</td>
      <td>93120.0</td>
      <td>FR</td>
      <td>75 Rue Rateau, 93120 La Courneuve, France</td>
      <td>True</td>
    </tr>
    <tr>
      <th>9</th>
      <td>JdiOTGoSQXxFIXB5g</td>
      <td>Thiais</td>
      <td>94320.0</td>
      <td>FR</td>
      <td>15 Rue des Alouettes, 94320 Thiais, France</td>
      <td>True</td>
    </tr>
  </tbody>
</table>
</div>




```python
df_task_locations.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 399 entries, 0 to 398
    Data columns (total 6 columns):
     #   Column       Non-Null Count  Dtype 
    ---  ------       --------------  ----- 
     0   location_id  398 non-null    object
     1   city         398 non-null    object
     2   postal_code  399 non-null    object
     3   country      398 non-null    object
     4   address      398 non-null    object
     5   confirmed    398 non-null    object
    dtypes: object(6)
    memory usage: 18.8+ KB
    


```python
#Checking Postal_code format
df_task_locations.postal_code.unique()
```




    array([94000., 13014., 75018., 93400., 77700., 93110., 75003., 93160.,
           93120., 94320., 75001., 93600., 75008., 59777., 75011., 75010.,
           75007., 33130., 75015., 92000., 13001., 75016., 93100., 94150.,
           94400., 78590., 75009.,  6100., 91320., 69003., 92110., 93130.,
           77184., 75006., 75012., 75014., 94100., 94200., 38000., 75002.,
           78340., 13140., 67000., 92300., 92100., 75116., 75019., 93450.,
           94220., 93230., 92600., 69290., 92310., 69700., 74400., 69100.,
           95210.,    nan, 78360., 78140., 92270., 69007., 78150., 69150.,
           93500., 94800., 59100., 92800., 13002., 69002., 33000.,  6000.,
           91100., 44800., 38090., 92390., 95130., 26100., 59800., 78410.,
           33300., 75020., 95310., 78370., 44000., 38120., 94110., 93300.,
           78700., 33800., 77400., 76000., 92230., 91140., 92092., 92340.,
           93200., 77350., 59175., 75004., 69140., 92400., 75013., 94380.,
           35760., 92130., 92200., 93170., 77127., 94270., 67480., 94130.,
           69006., 78000., 93390., 59000., 95000., 95150., 14000., 92320.,
            6400., 92150., 77600., 40150., 77610., 75017., 31000., 94550.,
           94370., 67450., 13100., 69200., 95370., 93150., 74000., 78300.,
           95220., 37000.,  6700., 95100., 13010., 95500., 92220., 13011.,
            6160., 60000., 10150., 27380., 78955., 77090., 21000., 67200.,
           77190., 95240., 93117., 83400., 75005., 93350., 91700., 94310.,
           59155., 91600., 69500., 91300.])




```python
#Changing float to postal code
df_task_locations.postal_code=df_task_locations.postal_code.astype(str)
df_task_locations.postal_code=df_task_locations.postal_code.apply(lambda x: x.split('.')[0])
df_task_locations.postal_code=df_task_locations.postal_code.apply(lambda x: '0'+x if len(x)==4 else x)
df_task_locations.postal_code.unique()
```




    array(['94000', '13014', '75018', '93400', '77700', '93110', '75003',
           '93160', '93120', '94320', '75001', '93600', '75008', '59777',
           '75011', '75010', '75007', '33130', '75015', '92000', '13001',
           '75016', '93100', '94150', '94400', '78590', '75009', '06100',
           '91320', '69003', '92110', '93130', '77184', '75006', '75012',
           '75014', '94100', '94200', '38000', '75002', '78340', '13140',
           '67000', '92300', '92100', '75116', '75019', '93450', '94220',
           '93230', '92600', '69290', '92310', '69700', '74400', '69100',
           '95210', 'nan', '78360', '78140', '92270', '69007', '78150',
           '69150', '93500', '94800', '59100', '92800', '13002', '69002',
           '33000', '06000', '91100', '44800', '38090', '92390', '95130',
           '26100', '59800', '78410', '33300', '75020', '95310', '78370',
           '44000', '38120', '94110', '93300', '78700', '33800', '77400',
           '76000', '92230', '91140', '92092', '92340', '93200', '77350',
           '59175', '75004', '69140', '92400', '75013', '94380', '35760',
           '92130', '92200', '93170', '77127', '94270', '67480', '94130',
           '69006', '78000', '93390', '59000', '95000', '95150', '14000',
           '92320', '06400', '92150', '77600', '40150', '77610', '75017',
           '31000', '94550', '94370', '67450', '13100', '69200', '95370',
           '93150', '74000', '78300', '95220', '37000', '06700', '95100',
           '13010', '95500', '92220', '13011', '06160', '60000', '10150',
           '27380', '78955', '77090', '21000', '67200', '77190', '95240',
           '93117', '83400', '75005', '93350', '91700', '94310', '59155',
           '91600', '69500', '91300'], dtype=object)



Adding Latitide and longitude with the help of https://adresse.data.gouv.fr/api-doc/adresse


```python
!curl.exe POST -F data=@..\data\task_locations.csv -F postcode=postal_code   https://api-adresse.data.gouv.fr/search/csv/ > ..\data/result.csv
```

      % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                     Dload  Upload   Total   Spent    Left  Speed
    
      0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0
      0     0    0     0    0     0      0      0 --:--:--  0:00:01 --:--:--     0curl: (6) Could not resolve host: POST
    
      0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0
    100 35799    0     0  100 35799      0  35799  0:00:01  0:00:01 --:--:-- 30159
    100 51921    0 16122  100 35799   8061  17899  0:00:02  0:00:02 --:--:-- 24618
    100 84650    0 48851  100 35799  24425  17899  0:00:02  0:00:02 --:--:-- 28511
    100   98k    0 65220  100 35799  16305   8949  0:00:04  0:00:04 --:--:-- 25254
    100  138k    0  104k  100 35799  26632   8949  0:00:04  0:00:04 --:--:-- 33246
    


```python
%pip install chardet
```

    Collecting chardet
      Downloading chardet-5.1.0-py3-none-any.whl (199 kB)
    Installing collected packages: chardet
    Successfully installed chardet-5.1.0
    Note: you may need to restart the kernel to use updated packages.
    

      WARNING: The script chardetect.exe is installed in 'C:\Users\idrim\AppData\Local\Packages\PythonSoftwareFoundation.Python.3.9_qbz5n2kfra8p0\LocalCache\local-packages\Python39\Scripts' which is not on PATH.
      Consider adding this directory to PATH or, if you prefer to suppress this warning, use --no-warn-script-location.
    WARNING: You are using pip version 21.3.1; however, version 22.3.1 is available.
    You should consider upgrading via the 'C:\Users\idrim\AppData\Local\Microsoft\WindowsApps\PythonSoftwareFoundation.Python.3.9_qbz5n2kfra8p0\python.exe -m pip install --upgrade pip' command.
    


```python
import chardet

with open('..\\data\\result.csv', 'rb') as f:
    enc = chardet.detect(f.read())  

df_task_locations_coordonates=pd.read_csv('..\\data\\result.csv', encoding = enc['encoding'])
```

Second way to fetch data with python



```python
%pip install requests
```

    Collecting requests
      Using cached requests-2.28.1-py3-none-any.whl (62 kB)
    Collecting charset-normalizer<3,>=2
      Using cached charset_normalizer-2.1.1-py3-none-any.whl (39 kB)
    Collecting idna<4,>=2.5
      Using cached idna-3.4-py3-none-any.whl (61 kB)
    Collecting certifi>=2017.4.17
      Using cached certifi-2022.12.7-py3-none-any.whl (155 kB)
    Collecting urllib3<1.27,>=1.21.1
      Using cached urllib3-1.26.13-py2.py3-none-any.whl (140 kB)
    Installing collected packages: urllib3, idna, charset-normalizer, certifi, requests
    Successfully installed certifi-2022.12.7 charset-normalizer-2.1.1 idna-3.4 requests-2.28.1 urllib3-1.26.13
    Note: you may need to restart the kernel to use updated packages.
    

      WARNING: The script normalizer.exe is installed in 'C:\Users\idrim\AppData\Local\Packages\PythonSoftwareFoundation.Python.3.9_qbz5n2kfra8p0\LocalCache\local-packages\Python39\Scripts' which is not on PATH.
      Consider adding this directory to PATH or, if you prefer to suppress this warning, use --no-warn-script-location.
    WARNING: You are using pip version 21.3.1; however, version 22.3.1 is available.
    You should consider upgrading via the 'C:\Users\idrim\AppData\Local\Microsoft\WindowsApps\PythonSoftwareFoundation.Python.3.9_qbz5n2kfra8p0\python.exe -m pip install --upgrade pip' command.
    


```python
import requests
import io

with open('..\\data\\task_locations.csv', 'rb') as fd: 
    payload = fd.read() 
arguments= {'postcode':'postal_code'}
r = requests.post("https://api-adresse.data.gouv.fr/search/csv/" ,  params=arguments, files={'data':payload})

df_task_locations_coordonates = pd.read_csv(io.StringIO(r.content.decode('utf-8')))



```


```python
#Kepping just coordinates and key
df_task_locations_coordonates=df_task_locations_coordonates[['location_id', 'latitude', 'longitude']]

```


```python
df_task_locations=df_task_locations.merge(df_task_locations_coordonates, on='location_id', how='left')
```


```python
df_task_locations
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
      <th>location_id</th>
      <th>city</th>
      <th>postal_code</th>
      <th>country</th>
      <th>address</th>
      <th>confirmed</th>
      <th>latitude</th>
      <th>longitude</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0OPfGunr3HYLwRKhE</td>
      <td>Créteil</td>
      <td>94000</td>
      <td>FR</td>
      <td>Av. de la France libre, 94000 Créteil, France</td>
      <td>True</td>
      <td>48.779265</td>
      <td>2.454933</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1MqyswOr2XSJhXdBD</td>
      <td>Marseille</td>
      <td>13014</td>
      <td>FR</td>
      <td>Av. du Marché national, 13014 Marseille, France</td>
      <td>True</td>
      <td>43.338663</td>
      <td>5.381507</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2M3flfoHbKJjr3Gxq</td>
      <td>Paris</td>
      <td>75018</td>
      <td>FR</td>
      <td>189 Rue d'Aubervilliers, 75018 Paris, France</td>
      <td>True</td>
      <td>48.896641</td>
      <td>2.370134</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3EbwFmGD1VJfHxWvI</td>
      <td>Saint-Ouen</td>
      <td>93400</td>
      <td>FR</td>
      <td>14 Rue Palouzié, 93400 Saint-Ouen, France</td>
      <td>True</td>
      <td>48.904746</td>
      <td>2.327179</td>
    </tr>
    <tr>
      <th>4</th>
      <td>7AK5TQ8rnwwcKk5so</td>
      <td>Serris</td>
      <td>77700</td>
      <td>FR</td>
      <td>14 Cr du Danube, 77700 Serris, France</td>
      <td>True</td>
      <td>48.856501</td>
      <td>2.778984</td>
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
    </tr>
    <tr>
      <th>394</th>
      <td>OSfSFYu3qEUdlwgUG</td>
      <td>Lille</td>
      <td>59000</td>
      <td>FR</td>
      <td>45 Rue Nationale 39, 59000 Lille, France</td>
      <td>True</td>
      <td>50.633078</td>
      <td>3.053181</td>
    </tr>
    <tr>
      <th>395</th>
      <td>Q44UYtwiwkkqW4vub</td>
      <td>Paris</td>
      <td>75018</td>
      <td>FR</td>
      <td>23 Av. Junot Pavillon D, 75018 Paris, France</td>
      <td>True</td>
      <td>48.888295</td>
      <td>2.335460</td>
    </tr>
    <tr>
      <th>396</th>
      <td>jt5ABf5vmRVow4H62</td>
      <td>Bron</td>
      <td>69500</td>
      <td>FR</td>
      <td>209 Bd Pinel, 69500 Bron, France</td>
      <td>True</td>
      <td>45.731587</td>
      <td>4.888310</td>
    </tr>
    <tr>
      <th>397</th>
      <td>XN7aXUjASEUbGy0v3</td>
      <td>Massy</td>
      <td>91300</td>
      <td>FR</td>
      <td>16 Mail Atlantis, 91300 Massy, France</td>
      <td>True</td>
      <td>48.725088</td>
      <td>2.263425</td>
    </tr>
    <tr>
      <th>398</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>nan</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>399 rows × 8 columns</p>
</div>




#### 3-Checking Siders datas:</br>  
  <ul>
  <li>Data Type, Count lines
  <li>Changing float to postal code    
  <li>Unique for each columns 
  </ul> 




```python
df_siders = pd.read_json('..\data\siders.json')  
```


```python
df_siders.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 4000 entries, 0 to 3999
    Data columns (total 18 columns):
     #   Column                             Non-Null Count  Dtype  
    ---  ------                             --------------  -----  
     0   sider_id                           4000 non-null   object 
     1   sider_type                         4000 non-null   object 
     2   status                             4000 non-null   object 
     3   work_legal_status                  3958 non-null   object 
     4   blacklisted                        4000 non-null   bool   
     5   is_student                         2719 non-null   float64
     6   latitude                           2309 non-null   float64
     7   longitude                          2309 non-null   float64
     8   has_caces_5                        1816 non-null   float64
     9   caces_5_expiration_date            1080 non-null   object 
     10  driving_license_obtaining_date     661 non-null    object 
     11  driving_license_validation_status  626 non-null    object 
     12  transport_preference_max_distance  2321 non-null   float64
     13  city                               2309 non-null   object 
     14  postal_code                        2309 non-null   float64
     15  department                         2293 non-null   object 
     16  region                             2309 non-null   object 
     17  country_code                       2309 non-null   object 
    dtypes: bool(1), float64(6), object(11)
    memory usage: 535.3+ KB
    


```python
df_siders.head(10)
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
      <th>sider_id</th>
      <th>sider_type</th>
      <th>status</th>
      <th>work_legal_status</th>
      <th>blacklisted</th>
      <th>is_student</th>
      <th>latitude</th>
      <th>longitude</th>
      <th>has_caces_5</th>
      <th>caces_5_expiration_date</th>
      <th>driving_license_obtaining_date</th>
      <th>driving_license_validation_status</th>
      <th>transport_preference_max_distance</th>
      <th>city</th>
      <th>postal_code</th>
      <th>department</th>
      <th>region</th>
      <th>country_code</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>si_20s1O15gkVv7K4YIqd3U2Py1czH</td>
      <td>testExp</td>
      <td>ready_to_work</td>
      <td>temp</td>
      <td>False</td>
      <td>False</td>
      <td>48.867290</td>
      <td>2.409276</td>
      <td>True</td>
      <td>2022-10-13</td>
      <td>None</td>
      <td>None</td>
      <td>45.0</td>
      <td>Paris</td>
      <td>75020</td>
      <td>Île-de-France</td>
      <td>Département de Paris</td>
      <td>FR</td>
    </tr>
    <tr>
      <th>1</th>
      <td>si_270eTMBowbshEpPh2w39jsFjYqM</td>
      <td>testExp</td>
      <td>ready_to_work</td>
      <td>temp</td>
      <td>False</td>
      <td>True</td>
      <td>48.941637</td>
      <td>2.213820</td>
      <td>True</td>
      <td>2027-03-25</td>
      <td>2017-02-01</td>
      <td>Validate Document</td>
      <td>30.0</td>
      <td>Argenteuil</td>
      <td>95100</td>
      <td>Île-de-France</td>
      <td>Val-d'Oise</td>
      <td>FR</td>
    </tr>
    <tr>
      <th>2</th>
      <td>si_1RMfOIoRiod5Yb42nHdMfQzJ4Rc</td>
      <td>testExp</td>
      <td>not_ready_to_work</td>
      <td>None</td>
      <td>False</td>
      <td>False</td>
      <td>49.021405</td>
      <td>2.208714</td>
      <td>True</td>
      <td>2024-09-20</td>
      <td>None</td>
      <td>None</td>
      <td>30.0</td>
      <td>Taverny</td>
      <td>95150</td>
      <td>Île-de-France</td>
      <td>Val-d'Oise</td>
      <td>FR</td>
    </tr>
    <tr>
      <th>3</th>
      <td>si_2316YOEOSjGqV6VFlK4I3LX04Mo</td>
      <td>testExp</td>
      <td>ready_to_work</td>
      <td>temp</td>
      <td>False</td>
      <td>False</td>
      <td>49.019038</td>
      <td>2.146190</td>
      <td>True</td>
      <td>2027-02-12</td>
      <td>None</td>
      <td>None</td>
      <td>30.0</td>
      <td>Pierrelaye</td>
      <td>95480</td>
      <td>Île-de-France</td>
      <td>Val-d'Oise</td>
      <td>FR</td>
    </tr>
    <tr>
      <th>4</th>
      <td>si_1l6080XPiBfvCIZrSfWh3Z527XS</td>
      <td>testExp</td>
      <td>not_ready_to_work</td>
      <td>temp</td>
      <td>False</td>
      <td>True</td>
      <td>48.798461</td>
      <td>2.314788</td>
      <td>True</td>
      <td>2023-05-24</td>
      <td>None</td>
      <td>None</td>
      <td>30.0</td>
      <td>Bagneux</td>
      <td>92220</td>
      <td>Île-de-France</td>
      <td>Hauts-de-Seine</td>
      <td>FR</td>
    </tr>
    <tr>
      <th>5</th>
      <td>si_25kYg2R0R3fZ0HQ6VRQEIJn4eFh</td>
      <td>testExp</td>
      <td>not_ready_to_work</td>
      <td>temp</td>
      <td>False</td>
      <td>True</td>
      <td>49.423445</td>
      <td>2.081256</td>
      <td>True</td>
      <td>2027-02-24</td>
      <td>None</td>
      <td>None</td>
      <td>90.0</td>
      <td>Beauvais</td>
      <td>60000</td>
      <td>Hauts-de-France</td>
      <td>Oise</td>
      <td>FR</td>
    </tr>
    <tr>
      <th>6</th>
      <td>si_1Ycusa4w7S6RbOlFkcWTnIsWeYj</td>
      <td>testExp</td>
      <td>ready_to_work</td>
      <td>temp</td>
      <td>False</td>
      <td>False</td>
      <td>48.603937</td>
      <td>2.568664</td>
      <td>True</td>
      <td>2023-05-11</td>
      <td>2015-09-28</td>
      <td>Validated</td>
      <td>60.0</td>
      <td>Savigny-le-Temple</td>
      <td>77176</td>
      <td>Île-de-France</td>
      <td>Seine-et-Marne</td>
      <td>FR</td>
    </tr>
    <tr>
      <th>7</th>
      <td>si_1SPk1y6W8WabJusvLma98VdNM7T</td>
      <td>testExp</td>
      <td>ready_to_work</td>
      <td>temp</td>
      <td>False</td>
      <td>False</td>
      <td>49.001993</td>
      <td>2.065532</td>
      <td>True</td>
      <td>2026-07-09</td>
      <td>None</td>
      <td>None</td>
      <td>30.0</td>
      <td>Maurecourt</td>
      <td>78780</td>
      <td>Île-de-France</td>
      <td>Yvelines</td>
      <td>FR</td>
    </tr>
    <tr>
      <th>8</th>
      <td>YFaK4Wkn1vkq9X34T</td>
      <td>default</td>
      <td>ready_to_work</td>
      <td>temp</td>
      <td>False</td>
      <td>False</td>
      <td>48.835764</td>
      <td>2.725477</td>
      <td>True</td>
      <td>2023-02-08</td>
      <td>None</td>
      <td>None</td>
      <td>30.0</td>
      <td>Bussy-Saint-Georges</td>
      <td>77600</td>
      <td>Île-de-France</td>
      <td>Seine-et-Marne</td>
      <td>FR</td>
    </tr>
    <tr>
      <th>9</th>
      <td>si_25QVVk45pRi6nxk5AAl9zay2t3T</td>
      <td>testExp</td>
      <td>not_ready_to_work</td>
      <td>temp</td>
      <td>False</td>
      <td>True</td>
      <td>46.713109</td>
      <td>0.259101</td>
      <td>True</td>
      <td>2026-09-21</td>
      <td>None</td>
      <td>None</td>
      <td>45.0</td>
      <td>Chabournay</td>
      <td>86380</td>
      <td>Nouvelle-Aquitaine</td>
      <td>Vienne</td>
      <td>FR</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Changing float to string for postal code
df_siders.postal_code=df_siders.postal_code.astype(str)
df_siders.postal_code=df_siders.postal_code.apply(lambda x: x.split('.')[0])
df_siders.postal_code=df_siders.postal_code.apply(lambda x: '0'+x if len(x)==4 else x)
df_siders.postal_code.unique()
```




    array(['75020', '95100', '95150', '95480', '92220', '60000', '77176',
           '78780', '77600', '86380', '94550', '93130', '93400', '59000',
           '69003', '92000', '93120', '91170', '91100', '94140', '75018',
           '77460', '93600', '59287', '95870', '93700', '95540', '91270',
           '91350', '94190', '75012', '93100', '75015', '77290', '93300',
           '95610', '95310', '95210', '69200', '93290', '93270', '69007',
           '93240', '94200', '95230', '92230', '42400', '78370', '95200',
           '13012', '78150', '91160', '77400', '91580', '75017', '93800',
           '91520', '91330', '91200', '62232', '69500', '92340', '75016',
           '75019', '80090', '93410', '92100', '93200', '94130', '92700',
           '38670', '94270', '75010', '72000', '44410', '93140', '92110',
           '95500', '93150', '93420', '92160', '95400', '78510', '94360',
           '77410', '69008', '94310', '78400', '77550', '77100', '91300',
           '91600', '75011', '95170', '69100', '07100', '69360', '93160',
           '38080', '93380', '75013', '77000', '91700', '94100', '94600',
           '77200', '91240', '06000', '93250', '33150', '94470', '78220',
           '93170', '92390', '78260', '95120', '94400', '95140', '93390',
           '59800', '91000', '95800', '94000', '93000', '77190', '77230',
           '38110', '13006', '95190', '78380', '77310', '31500', '44600',
           '91480', '78200', '91150', '94240', '95490', '62114', '75014',
           '94500', '78000', '91940', '77160', 'nan', '13014', '69270',
           '67300', '59200', '95340', '95370', '77700', '75003', '78600',
           '42100', '89140', '38090', '13002', '78160', '94220', '33500',
           '94350', '95700', '78711', '91290', '91120', '93110', '92600',
           '13008', '77220', '78210', '94260', '59119', '95300', '13015',
           '62117', '94230', '92140', '94430', '94510', '91230', '92420',
           '95630', '38550', '92250', '78730', '67000', '78650', '38780',
           '77127', '91260', '59430', '71250', '69190', '40090', '91210',
           '72100', '75009', '72500', '91130', '93330', '93450', '93220',
           '13320', '94460', '93430', '41000', '94700', '59370', '94120',
           '94340', '13010', '92370', '93360', '38950', '26000', '78520',
           '95280', '94450', '59700', '91220', '95460', '77340', '91800',
           '89210', '93190', '13500', '78250', '78480', '38070', '95000',
           '85610', '78500', '78700', '95110', '78130', '13800', '92320',
           '91620', '64140', '93350', '33310', '77570', '92500', '77130',
           '93210', '95240', '13011', '94110', '34080', '69390', '69005',
           '94210', '62100', '13003', '62290', '94320', '91860', '59650',
           '77140', '49170', '93500', '69210', '78590', '57460', '77144',
           '77680', '13127', '78955', '69600', '78300', '92260', '95290',
           '01200', '13680', '77500', '33340', '92400', '13013', '59260',
           '78170', '13120', '77181', '62219', '91070', '92290', '95130',
           '69800', '62590', '95470', '95420', '13700', '78710', '93440',
           '66140', '69140', '69009', '84100', '92240', '77350', '91380',
           '69700', '78340', '76260', '74960', '69230', '07800', '69150',
           '33300', '62300', '94380', '91360', '92800', '59790', '59140',
           '91560', '67120', '13310', '93320', '93310', '44220', '69780',
           '02400', '59155', '38200', '94800', '28700', '27140', '59450',
           '78280', '69320', '92380', '80080', '76130', '94290', '75001',
           '74410', '77270', '31700', '91080', '62640', '77320', '59223',
           '91430', '95330', '94370', '38300', '69120', '56100', '78410',
           '95520', '95380', '77420', '77540', '52100', '78190', '49300',
           '91540', '10200', '95160', '69330', '13001', '31200', '33600',
           '27420', '38460', '13250', '77186', '76170', '13004', '94150',
           '69420', '77330', '68480', '69680', '76000', '78990', '67800',
           '11100', '92170', '33440', '67100', '83000', '13100', '69300',
           '92130', '63170', '77210', '91400', '94880', '80000', '93260',
           '62200', '78110', '93230', '80100', '57270', '92300', '59300',
           '94300', '38100', '94410', '69001', '78310', '77114', '75007',
           '25000', '92120', '91460', '01700', '60300', '77380', '34070',
           '17520', '13009', '78570', '38120', '33800', '60800', '42000',
           '13140', '95750', '47500', '62440', '06400', '75004', '77170',
           '91250', '91410', '69490', '77750', '86000', '62160', '77185',
           '62970', '78550', '60110', '06200', '45320', '64121', '60180',
           '13300', '21000', '78800', '45380', '78320', '75006', '95270',
           '59150', '95350', '94170', '91190', '94390', '77166', '40530',
           '57000', '33530', '38290', '35160', '91390', '69370', '69560',
           '77120', '60280', '13005', '69740', '33560', '59420', '69130',
           '87520', '62400', '44200', '59163', '74160', '33130', '59164',
           '89100', '91420', '45400', '16220', '38121', '69510', '92150',
           '13130', '75002', '77390', '42150', '02100', '95600', '95880',
           '42800', '92190', '31000', '49100', '92210', '93460', '33400',
           '83470', '75008', '69290', '93340', '33160', '69004', '78100',
           '75116', '54000', '69006', '59320', '34090', '75005', '33270',
           '78420', '22100', '60500', '69260', '76160', '69730', '06110',
           '22800', '78230', '44100', '77370', '59130', '44000', '77184',
           '38000', '69650', '83600', '59175', '92310', '83640', '38920',
           '77640', '45100', '94160', '95390', '77860', '77260', '77590',
           '81300', '91370', '77610', '76520', '77360', '78180', '92360',
           '69002', '54150', '13016', '95360', '51100', '28250', '13007',
           '92200', '44240', '74210', '77580', '64000', '13090', '95720'],
          dtype=object)




```python
for col in df_siders:
  print(col, df_siders[col].unique())
```

    sider_id ['si_20s1O15gkVv7K4YIqd3U2Py1czH' 'si_270eTMBowbshEpPh2w39jsFjYqM'
     'si_1RMfOIoRiod5Yb42nHdMfQzJ4Rc' ... '5y0H85A41f0TFGFlJ'
     '5y0vPHrlEEUh4gh30' '5Y32ZXGeceFusAv8o']
    sider_type ['testExp' 'default' 'contratDeGestion']
    status ['ready_to_work' 'not_ready_to_work']
    work_legal_status ['temp' None]
    blacklisted [False  True]
    is_student [ 0.  1. -1.]
    latitude [48.8672898 48.9416371 49.0214047 ... 48.7727918 48.9058951 48.8739607]
    longitude [2.4092765 2.2138198 2.208714  ... 2.5188427 2.3256751 2.3600063]
    has_caces_5 [ 1.  0. -1.]
    caces_5_expiration_date ['2022-10-13' '2027-03-25' '2024-09-20' '2027-02-12' '2023-05-24'
     '2027-02-24' '2023-05-11' '2026-07-09' '2023-02-08' '2026-09-21'
     '2024-02-12' '2026-02-18' '2025-07-10' '2026-12-10' '2024-06-27'
     '2025-10-05' '2027-03-31' '2022-06-30' '2025-06-19' '2024-11-28'
     '2022-07-11' '2026-09-24' '2022-04-28' '2025-07-08' '2026-07-16'
     '2020-08-27' '2021-11-12' '2025-06-04' '2025-03-11' '2025-06-18'
     '2026-03-24' '2026-02-09' '2025-01-10' '2024-01-15' '2026-09-04'
     '2023-05-25' '2026-01-20' '2024-09-06' '2021-11-21' '2022-01-03'
     '2026-10-18' '2024-12-26' '2022-03-22' '2024-03-14' '2024-06-19'
     '2027-04-23' '2022-06-09' '2025-12-09' '2024-12-05' '2024-11-08'
     '2026-07-26' '2024-08-15' '2025-01-07' '2023-03-09' '2026-12-17'
     '2025-02-28' '2024-11-22' '2025-07-23' '2024-10-10' '2023-12-28'
     '2024-10-03' '2024-08-24' '2026-12-23' '2023-02-09' '2024-10-18'
     '2025-07-03' '2023-03-23' '2024-06-06' '2024-09-02' '2027-02-28'
     '2027-04-01' '2025-01-14' '2026-07-01' '2027-04-05' '2025-06-05'
     '2022-11-20' '2024-12-29' '2024-08-21' '2025-07-30' '2024-07-12'
     '2022-09-03' '2024-05-23' '2027-02-14' '2020-04-11' '2026-06-01'
     '2012-03-05' '2025-06-26' '2020-11-15' '2023-08-10' '2024-07-26'
     '2026-10-22' '2023-08-18' '2022-02-24' '2025-05-29' '2024-08-30'
     '2020-06-26' '2026-12-19' '2021-06-21' '2027-05-04' '2027-02-10'
     '2025-07-16' '2025-08-21' '2025-09-10' '2026-01-28' '2024-06-14'
     '2021-07-25' '2024-04-18' '2024-10-24' '2026-05-28' '2024-06-25'
     '2000-07-01' '2026-11-08' '2024-07-04' '2023-06-15' '2025-05-18'
     '2025-08-26' '2024-06-13' '2025-09-25' '2024-11-15' '2026-06-03'
     '2025-12-10' '2023-10-05' '2024-08-29' '2026-04-19' '2026-11-05'
     '2026-07-15' '2026-06-09' '2020-05-20' '2025-06-30' '2023-01-31'
     '2026-03-11' '2024-09-16' '2024-11-07' '2023-02-10' '2021-09-01'
     '2023-08-22' '2023-06-22' '2027-06-24' '2027-07-30' '2022-09-07'
     '2025-12-21' '2027-04-16' '2026-10-20' '2021-10-21' '2025-11-19'
     '2021-09-07' '2024-01-24' '2025-10-20' '2023-11-20' '2019-09-04'
     '2026-04-05' '2022-04-12' '2024-09-27' '2026-03-15' '2025-09-15'
     '2025-12-08' '2026-10-21' '2024-07-17' '2027-03-05' '2024-07-08'
     '2024-02-24' '2026-02-15' '2025-06-08' '1974-05-28' '2024-05-28'
     '2021-06-24' '2025-11-06' '2025-07-04' '2024-06-11' '2024-06-29'
     '2025-08-08' '2026-07-23' '2027-05-06' '2022-12-01' '2026-08-12'
     '2026-02-20' '2024-03-27' '2024-01-27' '2023-07-05' '2025-01-17'
     '2025-10-15' '2019-04-10' '2026-10-10' '2026-09-22' '2026-08-05'
     '2024-12-11' '2025-01-24' '2024-07-11' '2027-04-09' '2027-03-09'
     '2026-05-06' '2026-06-04' '2023-07-09' '2023-03-15' '2023-07-26'
     '2025-10-07' '2020-12-10' '2024-03-19' '2025-10-08' '2023-03-21'
     '2022-04-25' '2025-12-07' '2025-06-12' '2026-04-09' '2026-02-25'
     '2026-01-18' '2021-06-30' '2025-11-20' '2025-08-19' '2022-09-29'
     '2026-05-03' '2026-09-23' '2025-12-13' '2027-02-09' '2027-04-14'
     '2027-06-18' '2025-11-08' '2021-01-29' '2020-01-31' '2025-01-09'
     '2023-04-17' '2025-11-24' '2027-02-01' '2023-05-16' '2023-09-20'
     '2023-02-11' '2025-09-03' '2017-01-11' '2022-01-19' '2022-07-07'
     '2015-09-14' '2024-05-09' '2025-01-04' '2020-06-28' '2024-01-10'
     '2026-08-13' '2026-06-10' '2023-10-10' '2022-12-25' '2025-01-30'
     '2018-04-13' '2024-11-04' '2023-05-07' '2025-11-26' '2022-12-27'
     '2026-03-29' '2027-03-08' '2022-11-23' '2019-07-15' '2025-02-05'
     '2025-05-21' '2026-03-23' '2025-06-16' '2024-10-20' '2026-02-01'
     '2022-01-30' '2026-01-26' '2021-05-23' '2024-06-07' '2025-06-15'
     '2020-06-15' '2023-03-29' '2021-10-07' '2026-11-04' '2000-01-20'
     '2021-10-12' '2023-05-08' '2027-03-17' '2025-12-04' '2021-05-28'
     '1993-09-19' '2026-04-20' '2024-07-19' '2020-11-11' '2023-08-20'
     '2023-07-23' '2026-09-02' '2025-05-22' '2026-02-03' '2026-01-29'
     '2027-05-20' '2025-11-12' '2024-04-04' '2025-09-18' '2024-08-28'
     '2021-03-13' '2021-09-29' '2024-01-09' '2024-08-08' '2027-07-01'
     '2024-09-13' '2024-08-02' '2026-02-04' '2023-11-03' '2026-04-15'
     '2026-07-02' '2024-06-28' '2025-11-30' '2022-04-06' '2023-01-01'
     '2023-01-24' '2025-08-06' '2024-07-03' '2026-12-13' '2025-08-27'
     '2023-03-24' '2027-06-22' '2025-04-06' '2023-05-22' '2024-07-05'
     '2027-01-28' '2024-07-02' '2026-05-31' '2026-12-16' '2026-03-20'
     '2022-08-02' '2026-06-23' '2026-02-11' '2024-06-04' '2025-06-11'
     '2023-07-06' '2026-09-17' '2026-08-18' '2026-01-21' '2027-02-18'
     '2025-06-25' '1982-05-03' '2026-01-14' '2025-05-13' '2025-11-09'
     '2024-05-27' '2026-02-28' '2025-02-20' '2022-02-20' '2026-05-27'
     '2023-02-20' '2024-09-03' '2023-02-21' '2025-11-25' '2026-12-12'
     '2024-03-22' '2024-05-10' '2024-09-26' '2024-08-23' '2025-12-03'
     '2026-09-30' '2020-04-06' '2023-09-22' '2025-10-04' '2025-07-18'
     '2027-05-12' '2027-04-24' '2025-06-23' '2022-09-20' '2024-07-09'
     '2023-10-02' '2026-03-31' '2021-02-25' '2027-03-01' '2027-03-11'
     '2027-03-14' '2027-02-25' '2025-10-11' '2008-11-23' '2026-03-18'
     '2021-11-24' '2026-01-11' '2025-09-12' '2025-10-31' '2022-12-08'
     '2025-02-13' '2025-05-31' '2022-11-10' '2025-10-22' '1993-06-16'
     '2024-11-18' '2026-02-05' '2025-10-18' '2020-09-03' '2026-05-13'
     '2026-03-04' '2019-07-14' '2024-08-22' '2023-08-03' '2022-07-20'
     '2022-03-16' '2019-01-09' '2026-12-05' '2023-01-10' '2020-01-21'
     '2026-01-27' '2023-07-13' '2021-05-13' '2024-02-08' '2023-10-03'
     '2023-04-12' '2023-04-15' '2021-04-22' '2023-06-14' '2024-06-21'
     '2026-06-02' '2022-06-29' '2022-03-07' '2026-05-18' '2024-03-17'
     '2024-04-17' '2023-04-24' '2026-03-02' '2023-07-18' '2023-07-20'
     '2023-02-12' '2023-09-21' '2024-12-20' '2026-10-07' '2026-05-02'
     '2026-06-29' '2027-05-27' '2025-01-16' '2027-01-14' '2025-03-06'
     '2026-02-02' '2024-08-09' '2026-04-21' '2023-06-13' '2025-05-27'
     '2024-01-31' '2026-09-09' '2025-12-14' '2027-06-23' '2026-04-25'
     '2022-10-26' '2024-10-29' '2026-06-30' '2021-07-26' '2026-02-08'
     '2022-02-17' '2023-11-09' '2026-08-11' '2024-06-20' '2026-01-10'
     '2025-08-15' '2026-10-14' '2025-07-25' '2023-12-27' '2025-11-05'
     '2025-09-17' '1994-07-11' '2025-11-01' '2026-01-17' '2023-11-11'
     '2024-02-11' '2023-10-19' '2022-08-30' '2027-04-22' '2021-06-14'
     '2025-09-20' '2022-06-07' '2027-03-30' '2026-12-15' '2026-03-26'
     '2027-05-07' '2025-09-02' '2026-11-29' '1994-06-11' '2024-02-22'
     '1977-09-22' '2024-10-02' '2023-03-19' '2023-01-06' '2024-03-28'
     '2024-06-12' '2022-08-31' '2025-06-09' '2023-05-13' '2023-11-24'
     '1980-09-07' '2023-01-09' '2026-07-11' '2025-01-01' '2022-11-24'
     '2023-05-09' '2025-04-05' '2023-08-23' '2026-04-13' '2024-11-29'
     '2027-02-21' '2025-12-02' '2023-11-30' '2025-10-09' '2026-09-12'
     '2024-05-22' '2024-03-21' '2023-02-28' '2025-12-23' '2021-03-11'
     '2024-07-18' '2023-09-03' '2023-03-22' '2024-12-30' '2026-01-03'
     '2024-09-30' '2024-03-12' '2025-07-29' '2021-07-21' '2022-03-14'
     '2024-03-07' '2025-12-31' '2023-11-14' '2023-05-17' '2021-12-12'
     '2026-03-27' '2025-07-06' '2026-03-10' '2025-07-15' '2026-04-23'
     '2024-04-05' '2021-12-06' '2022-10-24' '2023-02-15' '2023-01-05'
     '2026-04-22' '2024-09-12' '2021-06-08' '2023-02-17' '2021-06-17'
     '2020-12-03' '2024-04-03' '2023-09-13' '2025-09-29' '2026-09-14'
     '2024-05-31' '2024-01-17' '2024-11-03' '2024-11-26' '2026-03-12'
     '2024-07-14' '2027-04-06' '2026-09-16' '2026-03-25' '2023-07-15'
     '2026-06-25' '2024-04-09' '2024-01-26' '2025-01-19' '2026-05-04'
     '2021-12-08' '2020-06-25' '2024-11-14' '2027-05-11' '2027-01-07'
     '2026-10-13' '2021-07-29' '1975-01-27' '2025-09-13' '2023-11-21'
     '2024-05-14' '2025-02-21' '2020-09-23' '2025-04-07' '2024-03-13'
     '2025-06-28' '2021-12-02' '2025-09-04' '2024-11-20' '2025-07-20'
     '2022-03-02' '2025-02-25' '2025-03-21' '2024-10-28' '2024-08-16'
     '2027-01-02' '2027-03-03' '2024-11-30' '2026-02-07' '1991-04-26'
     '2021-06-02' '2021-10-20' '2008-09-07' '2024-02-15' '2022-10-20'
     '2024-10-23' '2023-11-27' '2025-08-12' '2025-02-24' '2025-05-26'
     '2021-07-22' '2025-10-06' '2022-01-12' '2022-07-27' '2025-02-26'
     '1991-09-30' '2025-05-15' '2022-06-08' '2023-08-28' '2022-03-03'
     '2024-07-13' '2020-05-09' '2026-01-07' '2023-06-20' '2023-01-23'
     '2011-02-23' '2022-02-10' '2014-10-07' '2023-04-19' '2021-09-30'
     '2026-06-17' '2026-03-19' '2024-12-27' '2024-02-26' '2021-11-03'
     '2026-08-15' '2027-03-02' '2023-05-23' '2024-05-21' '2021-01-31'
     '2024-07-24' '2023-04-27' '2024-05-02' '2024-11-27' '2026-08-27'
     '2024-01-18' '2027-06-30' '2014-05-04' '2024-12-13' '2023-10-11'
     '2026-03-09' '2025-09-24' '2025-11-27' '1971-11-27' '2024-02-29'
     '2025-08-31' '2025-12-17' '2023-04-10' '2022-05-19' '2023-06-07'
     '2026-02-24' '2026-02-16' '1974-04-20' '2027-01-17' '2021-09-09'
     '2025-08-16' '2025-02-06' '2024-08-20' '1996-01-09' '2015-09-25'
     '2024-10-30' '2027-03-18' '2023-08-29' '2024-12-21' '2003-05-08'
     '2023-07-04' '2022-03-08' '2021-02-14' '2025-06-02' '1980-12-30'
     '2027-07-26' '2024-06-01' '2026-12-06' '2021-11-25' '2024-09-25'
     '2024-11-06' '2025-04-28' '2026-06-05' '2023-08-21' '2025-11-18'
     '2022-11-22' '2027-04-12' '2027-02-11' '2023-07-10' '2022-06-25'
     '2023-05-29' '2024-02-07' '2026-07-08' '2023-01-25' '2027-07-18'
     '2025-11-03' '2026-10-27' '2004-04-02' '2026-06-14' '2024-09-19'
     '2025-08-18' '2023-11-22' '2023-09-24' '2025-03-10' '2022-03-31'
     '2026-12-09' '2027-01-20' '2025-08-04' '2024-01-06' '2024-01-25'
     '2023-03-30' '2025-08-24' '2026-06-13' '2025-08-20' '2024-10-08'
     '2026-03-17' '2026-03-21' '2022-05-22' '2026-04-18' '2025-02-04'
     '2002-04-02' '2024-10-17' '2024-10-05' '2025-08-09' '2026-12-03'
     '2027-04-15' '2024-03-15' '2022-07-05' '2023-04-01' '2025-11-22'
     '2026-04-08' '2020-08-20' '2021-07-04' '2025-07-17' '2025-10-29'
     '2024-05-17' '2026-05-11' '2026-09-20' '2026-02-21' '2025-01-13'
     '2026-09-10' '2022-06-05' '2021-02-26' '2023-12-21' '2021-08-22'
     '2021-04-28' '2025-07-28' '2026-07-25' '2025-02-27' '2024-04-10'
     '2023-07-11' '1986-01-07' '2024-04-24' '2026-11-15' '2025-01-06'
     '2026-05-23' '2026-03-08' '2024-08-01' '2025-06-24' '2027-04-07'
     '2023-12-14' '2023-10-08' '2022-07-18' '2024-10-15' '2023-10-18'
     '2023-04-26' '2023-08-15' '2024-11-21' '2025-07-09' '2022-02-02'
     '2026-10-15' '2023-07-02' '2026-12-24' '2026-10-02' '2026-08-25'
     '2023-07-16' '2025-02-19' '2025-09-14' '2023-06-29' '2026-11-11'
     '2023-11-01' '2026-08-04' '2024-12-01' '2022-10-12' '2023-11-16'
     '2024-03-31' None]
    driving_license_obtaining_date [None '2017-02-01' '2015-09-28' '2015-06-22' '2020-11-02' '2017-02-10'
     '2019-05-16' '2018-09-06' '1994-02-11' '2019-06-13' '2018-07-12'
     '2019-10-29' '1986-07-04' '2003-06-27' '2017-10-04' '2013-12-19'
     '2021-10-15' '2019-10-18' '2017-01-04' '2018-01-25' '2019-05-21'
     '2017-06-09' '2017-02-09' '2017-02-02' '2011-12-03' '2015-04-22'
     '2020-06-24' '1997-03-13' '2015-04-09' '2013-06-21' '2019-07-17'
     '2020-07-28' '2019-10-02' '2014-03-20' '1979-11-23' '2020-10-12'
     '2017-04-12' '2001-07-20' '2022-03-29' '1986-01-06' '2017-09-18'
     '2020-09-18' '2019-09-07' '2016-09-19' '2019-11-08' '2004-11-24'
     '2017-07-03' '2008-04-04' '1977-03-27' '2019-07-23' '2019-02-21'
     '2012-07-20' '2003-05-10' '2018-11-15' '2019-02-14' '2019-11-22'
     '2013-02-05' '2017-03-30' '2019-10-08' '2014-06-14' '2019-04-02'
     '2009-08-20' '2017-12-04' '2017-06-27' '2012-02-21' '2019-06-25'
     '2021-12-23' '2019-03-14' '2016-12-12' '2017-11-14' '2020-09-28'
     '2015-08-01' '2008-10-09' '1998-09-01' '2019-10-04' '2013-11-08'
     '1997-05-22' '2017-11-16' '2015-03-23' '2018-11-21' '2013-05-14'
     '2019-07-09' '2021-01-26' '2016-11-25' '2014-08-13' '2011-06-20'
     '2019-10-17' '2016-01-04' '2001-11-14' '2019-05-09' '2017-03-07'
     '2020-03-03' '2005-11-06' '2021-07-01' '2002-04-04' '1988-12-20'
     '2019-03-27' '2016-02-19' '2013-09-13' '2021-03-09' '2020-06-08'
     '2015-09-04' '2011-06-09' '2002-04-16' '2019-09-19' '2010-07-20'
     '2019-01-15' '2019-08-21' '2020-10-02' '2018-09-22' '2017-01-05'
     '2020-02-03' '2005-05-12' '1994-08-18' '2018-04-05' '2012-03-07'
     '1986-02-12' '2020-10-26' '2001-03-16' '2019-07-26' '2021-04-09'
     '2008-08-17' '2021-08-13' '2020-12-19' '2019-03-25' '2007-02-13'
     '2009-08-16' '2016-01-29' '2016-01-07' '2019-10-07' '2019-05-29'
     '2016-07-07' '2016-06-02' '2020-09-08' '2014-01-31' '2021-07-05'
     '2011-08-31' '2019-03-13' '2010-11-05' '2016-05-10' '2021-01-04'
     '2021-12-01' '2002-11-07' '2018-03-06' '1985-12-18' '2002-05-11'
     '2004-04-22' '2014-01-06' '2021-05-21' '2012-05-16' '2020-06-18'
     '2016-12-07' '2016-10-07' '2020-12-10' '2020-06-30' '2000-12-27'
     '1995-09-23' '2019-07-02' '2016-09-09' '2001-06-12' '2017-09-07'
     '2012-12-18' '2021-05-18' '2021-01-20' '2021-09-05' '2017-05-22'
     '2019-07-19' '2019-08-22' '2015-12-20' '2015-06-10' '2016-12-08'
     '2021-04-16' '2019-05-07' '1985-07-11' '2016-05-02' '2020-08-10'
     '2007-09-21' '2007-07-14' '2017-12-14' '2016-03-31' '1992-08-25'
     '2018-10-11' '2016-04-11' '2020-01-24' '2018-12-18' '2004-11-08'
     '2010-01-19' '2010-08-30' '2014-03-11' '2014-04-23' '1993-07-08'
     '2016-03-30' '2009-06-17' '2018-01-12' '2017-10-19' '2020-09-24'
     '2013-12-12' '2017-10-12' '2017-09-14' '2015-05-29' '2011-10-17'
     '2017-06-21' '1995-12-15' '2019-06-24' '2017-08-23' '2013-07-04'
     '2013-04-19' '2013-10-03' '2011-09-09' '2012-02-03' '2011-07-20'
     '2016-04-04' '2020-05-31' '2016-09-07' '2017-06-26' '2009-10-06'
     '2011-07-07' '2008-09-04' '2017-01-16' '2021-01-15' '2017-06-22'
     '2019-07-05' '2017-07-06' '2019-05-04' '2011-03-16' '2014-07-31'
     '2019-02-08' '2019-01-01' '2011-09-20' '2020-07-31' '2001-01-08'
     '2004-05-18' '2014-11-27' '1983-08-14' '2015-05-27' '2020-07-21'
     '2011-03-08' '2018-12-14' '2018-06-01' '2014-01-14' '2020-02-19'
     '2020-01-10' '2019-11-15' '2010-02-27' '2011-09-19' '2019-08-01'
     '2021-02-19' '2019-03-18' '2011-03-14' '2012-04-04' '2005-03-21'
     '2012-02-09' '2012-02-08' '2017-02-16' '2019-12-23' '2018-07-18'
     '2019-01-14' '2020-12-01' '2008-12-09' '2015-08-19' '2000-06-29'
     '2018-11-20' '2017-12-22' '1986-03-25' '2009-09-11' '2017-11-24'
     '2013-09-09' '2002-02-05' '2008-04-15' '2013-03-19' '2015-09-16'
     '2016-03-09' '1995-02-24' '2012-09-06' '2019-08-12' '1990-11-23'
     '2021-12-09' '2014-09-22' '2016-06-29' '2018-01-19' '2011-03-07'
     '1998-08-18' '2007-05-31' '2017-10-27' '2014-05-19' '2021-02-08'
     '2012-01-18' '1990-11-24' '1999-05-04' '2015-11-19' '2014-03-04'
     '2019-10-03' '2015-03-07' '2016-08-16' '2017-06-15' '2002-10-18'
     '2009-12-15' '2020-07-07' '1999-02-09' '2004-10-28' '2007-06-11'
     '2012-06-04' '2010-08-20' '2016-12-09' '2021-06-07' '2019-04-12'
     '2019-03-01' '2018-09-28' '2019-02-15' '2015-02-05' '2019-02-01'
     '2014-01-29' '2004-01-15' '2001-07-17' '2018-09-21' '2021-10-22'
     '2015-01-22' '2018-02-06' '1990-06-15' '2019-10-01' '2021-01-14'
     '2020-09-01' '2020-10-13' '2021-04-21' '2008-05-26' '2003-11-28'
     '2019-05-15' '2012-02-29' '2021-05-27' '2008-10-01' '2017-02-12'
     '2020-03-13' '2014-03-29' '2010-01-10' '2018-03-20' '2009-04-04'
     '2014-04-14' '2012-08-31' '2016-12-19' '2018-11-26' '2020-08-06'
     '2020-09-29' '2017-04-07' '2019-01-07' '2015-02-26' '2015-11-04'
     '2006-03-08' '2017-07-28' '2010-04-27' '2018-09-25' '2017-02-24'
     '2018-09-27' '2006-03-02' '2017-06-29' '2012-03-16' '2017-03-06'
     '2019-10-31' '2010-10-25' '2016-07-26' '1990-06-19' '2018-01-05'
     '1998-04-27' '2012-10-12' '2007-07-09' '2014-08-12' '2019-01-24'
     '1998-04-04' '2018-06-02' '2021-04-24' '2017-11-21' '2018-12-17'
     '2019-05-10' '2009-07-20' '2021-04-28' '2016-08-19' '2019-04-04'
     '2015-05-28' '2015-10-20' '2020-06-11' '2020-08-23' '2012-01-26'
     '2018-06-21' '2007-09-06' '2016-11-09' '2017-09-19' '2013-10-23'
     '1979-09-08' '2005-02-15' '2013-04-21' '1998-08-26' '2017-04-25'
     '2004-02-09' '2011-04-04' '2018-06-07' '2006-03-22' '2014-01-22'
     '2022-05-12' '2015-09-08' '2018-05-31' '2017-03-17' '2017-12-01'
     '2020-03-09' '2018-03-05' '2019-07-01' '2021-12-16' '2013-10-21'
     '2008-02-12' '2017-04-03' '2021-11-30' '2019-12-05' '2016-09-21'
     '2018-08-22' '2017-01-27' '2012-08-12' '2003-06-13' '2015-01-13'
     '1995-05-18' '2012-12-06' '2015-10-13' '2020-03-05' '1992-03-25'
     '2016-07-18' '2020-07-29' '2018-11-22' '2020-02-13' '2022-02-14'
     '2018-08-31' '1995-06-06' '2022-04-24' '2011-06-03' '2008-05-30'
     '2017-11-26' '2019-03-16' '1999-06-11' '2018-04-16' '2018-08-09'
     '2003-06-05' '2021-10-23' '1995-12-31' '1975-03-25' '2021-09-22'
     '2018-08-29' '2011-03-09' '2013-04-14' '2013-03-29' '2020-09-02'
     '2017-06-01' '2019-06-01' '2006-09-28' '2013-11-10' '2011-02-25'
     '2019-11-14' '2013-12-16' '2019-10-14' '2008-09-01' '2014-05-27'
     '2011-10-13' '2019-12-06' '2007-01-24' '2011-09-25' '2016-08-20'
     '2012-06-05' '2010-06-21' '2000-10-13' '2012-01-05' '2018-06-19'
     '2017-02-22' '2018-11-19' '2021-02-10' '2019-09-23' '2018-11-28'
     '2011-02-22' '2018-02-01' '2019-08-05' '2017-08-22' '2010-02-10'
     '2015-06-03' '2018-05-25' '2015-08-18' '1999-10-06' '2019-07-31'
     '1997-11-18' '2017-05-28' '2020-09-23' '2017-01-25' '2019-09-21'
     '2015-10-15' '1984-05-09' '1986-08-10' '2008-07-23' '2018-11-13'
     '2016-02-29' '2001-11-20' '2014-11-05' '2017-12-15' '2011-07-27'
     '2020-11-07' '2015-11-03' '1997-04-03' '2017-05-24' '2018-07-27'
     '2017-01-02' '2022-02-01' '2013-09-12' '2021-05-07' '2018-01-23'
     '2015-12-18' '2018-01-24' '1981-10-14' '2019-04-26' '2016-04-08'
     '1999-01-22' '2005-01-15' '2017-04-14' '2018-07-05' '1999-11-09'
     '2022-03-22' '2020-02-01' '2011-05-25' '2021-07-21' '2019-10-11'
     '2021-08-03' '2019-05-20' '2021-03-01' '2020-06-29' '2017-09-26'
     '2022-02-02' '2020-08-12' '2021-05-05' '2016-02-10' '2020-08-04'
     '2019-10-22' '2020-08-05' '2003-11-04' '2020-11-11' '2018-08-06'
     '2015-02-11' '2017-06-23' '1988-04-22' '2010-08-25' '2019-10-16'
     '2010-01-04' '2014-10-08' '1991-04-24' '2017-03-23' '2019-02-18'
     '2013-10-08' '2011-01-20' '1997-12-19' '2010-10-14' '2016-11-23'
     '2019-06-14' '2020-10-05' '2010-09-17' '2009-09-30' '1992-05-06'
     '2016-07-16' '2009-03-17' '2005-03-22' '2003-10-03' '2018-08-13'
     '2018-06-29' '2014-08-14' '2017-10-16' '2010-10-20' '2009-01-15'
     '2017-09-20' '2013-05-22' '2015-06-15' '1998-02-11' '2007-12-31'
     '2010-10-27' '2013-10-29' '2012-05-23' '1989-04-24' '2008-02-29'
     '2020-12-28' '2018-06-15' '2019-06-19' '1995-10-14' '1998-05-05'
     '2018-05-15' '2020-01-08' '2016-02-15' '2018-02-28' '2017-07-04'
     '2020-07-02' '2020-06-01' '1997-07-17' '2019-02-04' '2017-05-15'
     '2017-04-11' '2019-11-02' '2016-10-12' '2008-12-01' '2015-08-17'
     '2019-02-28' '2012-02-04' '2016-11-03' '2020-08-03' '2018-09-20'
     '2016-09-28' '2018-12-21' '2018-04-04' '2015-07-28' '2013-06-26'
     '2019-09-24' '2014-10-31' '2020-10-10' '2006-04-12' '2011-04-21'
     '2016-10-14' '2000-06-09' '1970-10-22' '2016-05-11']
    driving_license_validation_status [None 'Validate Document' 'Validated' 'Refused']
    transport_preference_max_distance [ 45.  30.  90.  60. 120.  15.  nan]
    city ['Paris' 'Argenteuil' 'Taverny' 'Pierrelaye' 'Bagneux' 'Beauvais'
     'Savigny-le-Temple' 'Maurecourt' 'Bussy-Saint-Georges' 'Chabournay'
     'Chevilly-Larue' 'Noisy-le-Sec' 'Saint-Ouen' 'Lille' 'Lyon' 'Nanterre'
     'La Courneuve' 'Viry-Châtillon' 'Corbeil-Essonnes' 'Alfortville'
     'Souppes-sur-Loing' 'Aulnay-sous-Bois' 'Guesnain' 'Bezons' 'Drancy'
     'Méry-sur-Oise' 'Vigneux-sur-Seine' 'Grigny' 'Villeneuve-Saint-Georges'
     'Montreuil' 'Mitry-Mory' 'Aubervilliers' 'Éragny' "Saint-Ouen-l'Aumône"
     'Saint-Gratien' 'Vénissieux' 'Tremblay-en-France' 'Sevran' 'Stains'
     'Ivry-sur-Seine' 'Soisy-sous-Montmorency' 'Gennevilliers' 'Saint-Chamond'
     'Plaisir' 'Sarcelles' 'Marseille' 'Le Chesnay' 'Longjumeau'
     'Lagny-sur-Marne' 'Étréchy' 'Épinay-sur-Seine' 'Égly' 'Yerres'
     'Athis-Mons' 'Annezin' 'Bron' 'Bourg-la-Reine' 'Amiens' 'Vaujours'
     'Boulogne-Billancourt' 'Saint-Denis' 'Nogent-sur-Marne' 'Colombes'
     'Chasse-sur-Rhône' 'Le Kremlin-Bicêtre' 'Le Mans' 'Herbignac' 'Bondy'
     'Clichy' 'Gonesse' 'Le Blanc-Mesnil' 'Villepinte' 'Antony'
     'Villiers-le-Bel' 'Triel-sur-Seine' 'Bry-sur-Marne' 'Claye-Souilly'
     'Orly' 'Chatou' 'Moissy-Cramayel' 'Meaux' 'Massy' 'Savigny-sur-Orge'
     'Deuil-la-Barre' 'Villeurbanne' 'Annonay' 'Ternay' 'Noisy-le-Grand'
     "L'Isle-d'Abeau" 'Pierrefitte-sur-Seine' 'Melun'
     'Sainte-Geneviève-des-Bois' 'Saint-Maur-des-Fossés' 'Choisy-le-Roi'
     'Torcy' 'Saint-Michel-sur-Orge' 'Nice' 'Villemomble' 'Cenon'
     'Boissy-Saint-Léger' 'Viroflay' 'Bagnolet' 'Villeneuve-la-Garenne'
     'Achères' 'Ermont' 'Vitry-sur-Seine' 'Garges-lès-Gonesse'
     'Clichy-sous-Bois' 'Évry' 'Cergy' 'Créteil' 'Bobigny' 'Dammarie-les-Lys'
     'Thieux' 'Cessieu' 'Goussainville' 'Bougival' 'Saint-Fargeau-Ponthierry'
     'Toulouse' 'Saint-Nazaire' 'Quincy-sous-Sénart' 'Mantes-la-Jolie'
     'Étampes' "L'Haÿ-les-Roses" 'Longperrier' 'Vauréal' 'Sains-en-Gohelle'
     'Champigny-sur-Marne' 'Versailles' 'Les Ulis' 'Saint-Jean-de-Beauregard'
     'Provins' None 'Fontaines-sur-Saône' 'Schiltigheim' 'Tourcoing'
     'Bernes-sur-Oise' 'Saint-Alban-de-Roche' 'Montigny-lès-Cormeilles'
     'Chessy' 'Maisons-Laffitte' 'Saint-Étienne' 'Gisy-les-Nobles'
     'Villefontaine' 'Marly-le-Roi' 'Charenton-le-Pont' 'Libourne'
     'Villiers-sur-Marne' 'Roissy-en-France' 'Mantes-la-Ville' 'Arpajon'
     'Palaiseau' 'Rosny-sous-Bois' 'Asnières-sur-Seine' 'Tournan-en-Brie'
     "Saint-Cyr-l'École" 'Fresnes' 'Waziers' 'Pontoise' 'Brebières' 'Cachan'
     'Clamart' 'Chennevières-sur-Marne' 'La Queue-en-Brie' 'Vaucresson'
     'Mériel' 'Le Péage-de-Roussillon' 'La Garenne-Colombes' 'Sainte-Mesme'
     'Strasbourg' 'Beynes' 'Septème' 'Lieusaint' 'Juvisy-sur-Orge' 'Dunkerque'
     'Saint-Fons' 'Saint-Avit' 'Draveil' 'Montval-sur-Loir' 'Ris-Orangis'
     'Neuilly-sur-Marne' "L'Île-Saint-Denis" 'Gagny' 'Bouc-Bel-Air' 'Valenton'
     'Villetaneuse' 'Blois' 'Maisons-Alfort' 'Mons-en-Barœul'
     'Fontenay-sous-Bois' 'Joinville-le-Pont' 'Chaville' 'Neuilly-Plaisance'
     'Quaix-en-Chartreuse' 'Valence' 'Limay' 'Jouy-le-Moutier'
     'Limeil-Brévannes' 'Marcq-en-Barœul' 'Brétigny-sur-Orge' 'Ézanville'
     'Pontault-Combault' 'Brunoy' 'Brienon-sur-Armançon' 'Livry-Gargan'
     'Fleury-Mérogis' 'Martigues' 'Hardricourt' 'Verneuil-sur-Seine'
     'Saint-Quentin-Fallavier' 'Cugand' 'Sartrouville'
     'Conflans-Sainte-Honorine' 'Sannois' 'Les Mureaux' 'Istres' 'Châtillon'
     'Nozay' 'Billère' 'Le Bourget' 'Pomponne' 'Lormont' 'Château-Landon'
     'Rueil-Malmaison' 'Montereau-Fault-Yonne' 'Évry-Courcouronnes'
     'Cormeilles-en-Parisis' 'Arcueil' 'Montpellier' 'Vernaison' 'Calais'
     'Bailly-Romainvilliers' 'Nœux-les-Mines' 'Thiais' 'Épinay-sous-Sénart'
     "Villeneuve-d'Ascq" 'Nemours' 'Saint-Georges-sur-Loire' 'Pantin' 'Éveux'
     'Varennes-Jarcy' 'Noisy-le-Roi' 'Kerbach' 'Montévrain' 'Roissy-en-Brie'
     'Vitrolles' 'Carrières-sous-Poissy' 'Oullins' 'Poissy'
     'Fontenay-aux-Roses' "L'Isle-Adam" 'Valserhône' 'Lançon-Provence'
     'Chelles' 'Saint-Christoly-Médoc' 'Courbevoie' 'La Celle-Saint-Cloud'
     'Gardanne' 'Courtry' 'Longuenesse' 'Bondoufle' 'Châtenay-Malabry'
     'Franconville' 'Saint-Priest' 'Oignies' 'Vémars' 'Magny-en-Vexin'
     'Marignane' 'Dugny' 'Canet-en-Roussillon' 'Rillieux-la-Pape' 'Orange'
     'Malakoff' 'Le Mée-sur-Seine' 'Chilly-Mazarin' 'Givors'
     'Les Clayes-sous-Bois' 'Flocques' 'Annecy' 'Saint-Genis-Laval'
     'La Voulte-sur-Rhône' 'Décines-Charpieu' 'Bordeaux' 'Lens' 'Eu'
     'Bonneuil-sur-Marne' 'Épinay-sur-Orge' 'Puteaux' 'Ronchin' 'Crosne'
     'Molsheim' 'Saint-Martin-de-Crau' 'Les Pavillons-sous-Bois'
     'Le Pré-Saint-Gervais' 'Couëron' 'Mions' 'Brasles' 'Faches-Thumesnil'
     'Vienne' 'Villejuif' 'Umpeau' 'Gisors' 'Sin-le-Noble' 'Guyancourt'
     'Feyzin' 'Garches' 'Mont-Saint-Aignan' 'Villeneuve-le-Roi' 'Saint-Jorioz'
     'Persan' 'Villeparisis' 'Blagnac' 'Montigny-en-Gohelle' 'Montgeron'
     'La Ferté-Gaucher' 'Roncq' 'Vauhallan' 'Domont' 'Sucy-en-Brie'
     'Bourgoin-Jallieu' 'Vaulx-en-Velin' 'Lorient' 'Aubergenville' 'Osny'
     'Marly-la-ville' 'Champs-sur-Marne' 'Bernay-Vilbert' 'Saint-Dizier'
     'Trappes' 'Cholet' 'Mennecy' 'Ailleville' 'Montmorency' 'Meyzieu'
     'Pessac' 'Authevernes' 'Chamagnieu' 'Saint-Chamas' 'Noisiel' 'Lillebonne'
     'Rungis' 'Ampuis' 'Ozoir-la-Ferrière' 'Bouxwiller' 'Chassieu' 'Rouen'
     'Élancourt' 'Bischheim' 'Narbonne' 'Vanves' 'Villabé'
     'Ambarès-et-Lagrave' 'Toulon' 'Aix-en-Provence' 'Louvres'
     'Caluire-et-Cuire' 'Issy-les-Moulineaux' 'Aubière' 'Avon' 'Orsay'
     'Noiseau' 'Les Lilas' 'Boulogne-sur-Mer' 'Le Vésinet' 'Romainville'
     'Abbeville' 'Uckange' 'Levallois-Perret' 'Valenciennes' 'Vincennes'
     'Grenoble' 'Saint-Maurice' 'Maurepas' 'Hermé' 'Besançon' 'Montrouge'
     'Hœnheim' 'Marcoussis' 'Beynost' 'Senlis' 'Combs-la-Ville'
     'Brie-Sous-Archiac' 'Chanteloup-les-Vignes' 'Saint-Egrève'
     'Crépy-en-Valois' 'Miramas' 'Chars' 'Condezaygues' 'Harnes' 'Cannes'
     'Brie-Comte-Robert' 'Tigery' 'Dourdan' 'Vindry-sur-Turdine'
     'Saint-Cyr-sur-Morin' 'Poitiers' 'Bully-les-Mines' 'Lognes'
     'Courcelles-lès-Lens' 'Houdan' 'Méru' 'Courtenay' 'Montardon'
     'Nogent-sur-Oise' 'Salon-de-Provence' 'Dijon' 'Houilles'
     'La Chapelle-Saint-Mesmin' 'La Verrière' 'Dampmart' 'Luzarches'
     'Coignières' 'Wattrelos' 'Saint-Brice-sous-Forêt' 'Le Perreux-sur-Marne'
     'Gif-sur-Yvette' 'Paray-Vieille-Poste' 'Vaux-le-Pénil'
     'Évry-Grégy-sur-Yerre' 'Labenne' 'Metz' 'Bassens' 'La Verpillière'
     'Talensac' 'Morsang-sur-Orge' "Saint-Didier-au-Mont-d'Or"
     'Saint-Cyr-sur-le-Rhône' 'Vourles' 'Coulommiers' 'Margny-lès-Compiègne'
     'Genas' 'Carbon-Blanc' 'Mouvaux' 'Écully' 'Veyrac' 'Béthune' 'Nantes'
     "Condé-sur-l'Escaut" 'Saint-Julien-en-Genevois' 'Bègles' 'Marpent' 'Sens'
     'Morangis' 'Fleury-les-Aubrais' 'Montbron' "Chonas-l'Amballan"
     'Soucieu-en-Jarrest' 'Suresnes' "Berre-l'Étang" 'Jouy-sur-Morin'
     'Guignes' 'La Ricamarie' 'Saint-Quentin' 'Eaubonne' 'Enghien-les-Bains'
     'Arnouville' 'Saint-Joseph' 'Le Chesnay-Rocquencourt' 'Meudon' 'Angers'
     'Saint-Cloud' 'Gournay-sur-Marne' 'Talence'
     'Saint-Maximin-la-Sainte-Baume' 'Trept' 'Grézieu-la-Varenne' 'Le Raincy'
     'Saint-Médard-en-Jalles' 'Saint-Germain-en-Laye' 'Nancy'
     'Hallennes-lez-Haubourdin' 'Bouliac' 'Carrières-sur-Seine' 'Quévert'
     'Chantilly' 'Charbonnières-les-Bains' 'Darnétal' 'Genay' 'Le Cannet'
     'Quintin' 'Le Pecq' 'Grisy-Suisnes' 'Maison-Rouge' 'Lambersart'
     'Émerainville' 'Quincieux' 'Fréjus' 'Templemars' 'Sèvres'
     'Saint-Zacharie' 'Crolles' 'Jouarre' 'Orléans' 'Saint-Mandé' 'Saint-Prix'
     'Quincy-Voisins' 'La Ferté-sous-Jouarre' 'Bois-le-Roi' 'Graulhet'
     'Verrières-le-Buisson' 'La Houssaye-en-Brie' 'Saint-Mard' 'Boos'
     'Vaires-sur-Marne' 'Montigny-le-Bretonneux' 'Val de Briey' 'Montmagny'
     'Nangis' 'Reims' 'Louvilliers-lès-Perche' 'Neuilly-sur-Seine'
     'La Chapelle-sur-Erdre' 'Faverges' 'Guérard' 'Pau' 'Thorigny-sur-Marne'
     'Le Mesnil-Aubry']
    postal_code ['75020' '95100' '95150' '95480' '92220' '60000' '77176' '78780' '77600'
     '86380' '94550' '93130' '93400' '59000' '69003' '92000' '93120' '91170'
     '91100' '94140' '75018' '77460' '93600' '59287' '95870' '93700' '95540'
     '91270' '91350' '94190' '75012' '93100' '75015' '77290' '93300' '95610'
     '95310' '95210' '69200' '93290' '93270' '69007' '93240' '94200' '95230'
     '92230' '42400' '78370' '95200' '13012' '78150' '91160' '77400' '91580'
     '75017' '93800' '91520' '91330' '91200' '62232' '69500' '92340' '75016'
     '75019' '80090' '93410' '92100' '93200' '94130' '92700' '38670' '94270'
     '75010' '72000' '44410' '93140' '92110' '95500' '93150' '93420' '92160'
     '95400' '78510' '94360' '77410' '69008' '94310' '78400' '77550' '77100'
     '91300' '91600' '75011' '95170' '69100' '07100' '69360' '93160' '38080'
     '93380' '75013' '77000' '91700' '94100' '94600' '77200' '91240' '06000'
     '93250' '33150' '94470' '78220' '93170' '92390' '78260' '95120' '94400'
     '95140' '93390' '59800' '91000' '95800' '94000' '93000' '77190' '77230'
     '38110' '13006' '95190' '78380' '77310' '31500' '44600' '91480' '78200'
     '91150' '94240' '95490' '62114' '75014' '94500' '78000' '91940' '77160'
     'nan' '13014' '69270' '67300' '59200' '95340' '95370' '77700' '75003'
     '78600' '42100' '89140' '38090' '13002' '78160' '94220' '33500' '94350'
     '95700' '78711' '91290' '91120' '93110' '92600' '13008' '77220' '78210'
     '94260' '59119' '95300' '13015' '62117' '94230' '92140' '94430' '94510'
     '91230' '92420' '95630' '38550' '92250' '78730' '67000' '78650' '38780'
     '77127' '91260' '59430' '71250' '69190' '40090' '91210' '72100' '75009'
     '72500' '91130' '93330' '93450' '93220' '13320' '94460' '93430' '41000'
     '94700' '59370' '94120' '94340' '13010' '92370' '93360' '38950' '26000'
     '78520' '95280' '94450' '59700' '91220' '95460' '77340' '91800' '89210'
     '93190' '13500' '78250' '78480' '38070' '95000' '85610' '78500' '78700'
     '95110' '78130' '13800' '92320' '91620' '64140' '93350' '33310' '77570'
     '92500' '77130' '93210' '95240' '13011' '94110' '34080' '69390' '69005'
     '94210' '62100' '13003' '62290' '94320' '91860' '59650' '77140' '49170'
     '93500' '69210' '78590' '57460' '77144' '77680' '13127' '78955' '69600'
     '78300' '92260' '95290' '01200' '13680' '77500' '33340' '92400' '13013'
     '59260' '78170' '13120' '77181' '62219' '91070' '92290' '95130' '69800'
     '62590' '95470' '95420' '13700' '78710' '93440' '66140' '69140' '69009'
     '84100' '92240' '77350' '91380' '69700' '78340' '76260' '74960' '69230'
     '07800' '69150' '33300' '62300' '94380' '91360' '92800' '59790' '59140'
     '91560' '67120' '13310' '93320' '93310' '44220' '69780' '02400' '59155'
     '38200' '94800' '28700' '27140' '59450' '78280' '69320' '92380' '80080'
     '76130' '94290' '75001' '74410' '77270' '31700' '91080' '62640' '77320'
     '59223' '91430' '95330' '94370' '38300' '69120' '56100' '78410' '95520'
     '95380' '77420' '77540' '52100' '78190' '49300' '91540' '10200' '95160'
     '69330' '13001' '31200' '33600' '27420' '38460' '13250' '77186' '76170'
     '13004' '94150' '69420' '77330' '68480' '69680' '76000' '78990' '67800'
     '11100' '92170' '33440' '67100' '83000' '13100' '69300' '92130' '63170'
     '77210' '91400' '94880' '80000' '93260' '62200' '78110' '93230' '80100'
     '57270' '92300' '59300' '94300' '38100' '94410' '69001' '78310' '77114'
     '75007' '25000' '92120' '91460' '01700' '60300' '77380' '34070' '17520'
     '13009' '78570' '38120' '33800' '60800' '42000' '13140' '95750' '47500'
     '62440' '06400' '75004' '77170' '91250' '91410' '69490' '77750' '86000'
     '62160' '77185' '62970' '78550' '60110' '06200' '45320' '64121' '60180'
     '13300' '21000' '78800' '45380' '78320' '75006' '95270' '59150' '95350'
     '94170' '91190' '94390' '77166' '40530' '57000' '33530' '38290' '35160'
     '91390' '69370' '69560' '77120' '60280' '13005' '69740' '33560' '59420'
     '69130' '87520' '62400' '44200' '59163' '74160' '33130' '59164' '89100'
     '91420' '45400' '16220' '38121' '69510' '92150' '13130' '75002' '77390'
     '42150' '02100' '95600' '95880' '42800' '92190' '31000' '49100' '92210'
     '93460' '33400' '83470' '75008' '69290' '93340' '33160' '69004' '78100'
     '75116' '54000' '69006' '59320' '34090' '75005' '33270' '78420' '22100'
     '60500' '69260' '76160' '69730' '06110' '22800' '78230' '44100' '77370'
     '59130' '44000' '77184' '38000' '69650' '83600' '59175' '92310' '83640'
     '38920' '77640' '45100' '94160' '95390' '77860' '77260' '77590' '81300'
     '91370' '77610' '76520' '77360' '78180' '92360' '69002' '54150' '13016'
     '95360' '51100' '28250' '13007' '92200' '44240' '74210' '77580' '64000'
     '13090' '95720']
    department ['Île-de-France' 'Hauts-de-France' 'Nouvelle-Aquitaine'
     'Auvergne-Rhône-Alpes' "Provence-Alpes-Côte d'Azur" None
     'Pays de la Loire' 'Paris' 'Occitanie' 'Grand Est'
     'Bourgogne-Franche-Comté' 'Centre-Val de Loire' 'Normandie' 'Bretagne'
     'Charente-Maritime' 'Landes']
    region ['Département de Paris' "Val-d'Oise" 'Hauts-de-Seine' 'Oise'
     'Seine-et-Marne' 'Yvelines' 'Vienne' 'Val-de-Marne' 'Seine-Saint-Denis'
     'Arrondissement de Paris' 'Nord' 'Rhône' 'Essonne' 'Loire'
     'Bouches-du-Rhône' 'Pas-de-Calais' 'Somme' 'Isère' 'Sarthe'
     'Loire-Atlantique' 'Île-de-France' 'Ardèche' 'Alpes-Maritimes' 'Gironde'
     'Haute-Garonne' None 'Bas-Rhin' 'Yonne' 'Saône-et-Loire' 'Landes'
     'Loir-et-Cher' 'Drôme' 'Vendée' 'Pyrénées-Atlantiques' 'Hérault'
     'Maine-et-Loire' 'Moselle' 'Ain' 'Pyrénées-Orientales' 'Vaucluse'
     'Seine-Maritime' 'Haute-Savoie' 'Aisne' 'Eure-et-Loir' 'Eure' 'Morbihan'
     'Haute-Marne' 'Aube' 'Haut-Rhin' 'Aude' 'Var' 'Puy-de-Dôme' 'Doubs'
     'Nouvelle-Aquitaine' 'Lot-et-Garonne' 'Loiret' "Côte-d'Or"
     'Ille-et-Vilaine' 'Haute-Vienne' 'Charente' 'Meurthe-et-Moselle'
     "Côtes-d'Armor" 'Tarn' 'Marne']
    country_code ['FR' None]
    


```python
df_siders.driving_license_validation_status.unique()
```




    array([None, 'Validate Document', 'Validated', 'Refused'], dtype=object)



####  Number of siders without adress !!


```python
#Number of siders without adress
df_siders.city.isna().sum()
```




    1691




```python
df_siders.blacklisted.unique()
```




    array([False,  True])




#### 4-Checking tasks datas:</br>  
  <ul>
  <li>Data Type, Count lines
  </ul> 



```python
df_tasks = pd.read_json('data\\tasks.json')  
```


```python
df_tasks.head(10)
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
      <th>task_id</th>
      <th>status</th>
      <th>pricing_sider</th>
      <th>pricing_currency</th>
      <th>work_legal_status</th>
      <th>location_id</th>
      <th>start_date</th>
      <th>end_date</th>
      <th>require_caces_5</th>
      <th>require_driving_license</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>ta_JfJCVLRTYv92T6mOX</td>
      <td>submitted</td>
      <td>14.50</td>
      <td>EUR</td>
      <td>temp</td>
      <td>E5lbCiLISbeXnCUac</td>
      <td>2022-09-26 07:30:00.000000 +00:00</td>
      <td>2022-09-30 16:30:00.000000 +00:00</td>
      <td>True</td>
      <td>True</td>
    </tr>
    <tr>
      <th>1</th>
      <td>ta_CtPBrdZeumlL7p28K</td>
      <td>submitted</td>
      <td>13.19</td>
      <td>EUR</td>
      <td>temp</td>
      <td>jfGxIyMqW4mpuiGEj</td>
      <td>2022-09-19 07:00:00.000000 +00:00</td>
      <td>2022-12-19 16:00:00.000000 +00:00</td>
      <td>False</td>
      <td>True</td>
    </tr>
    <tr>
      <th>2</th>
      <td>ta_Wl547iLHpgLMOlciK</td>
      <td>submitted</td>
      <td>13.00</td>
      <td>EUR</td>
      <td>temp</td>
      <td>Q44UYtwiwkkqW4vub</td>
      <td>2022-09-10 17:00:00.000000 +00:00</td>
      <td>2022-09-10 21:00:00.000000 +00:00</td>
      <td>False</td>
      <td>True</td>
    </tr>
    <tr>
      <th>3</th>
      <td>ta_en7yXcpFgrFaClbjv</td>
      <td>submitted</td>
      <td>10.85</td>
      <td>EUR</td>
      <td>temp</td>
      <td>gc6HdosKSFwlNDdFu</td>
      <td>2022-09-10 15:45:00.000000 +00:00</td>
      <td>2022-09-10 18:00:00.000000 +00:00</td>
      <td>False</td>
      <td>True</td>
    </tr>
    <tr>
      <th>4</th>
      <td>ta_j7FNEpx0LqkLsT0nC</td>
      <td>submitted</td>
      <td>12.00</td>
      <td>EUR</td>
      <td>temp</td>
      <td>4exFJS3aS3cX0e7vt</td>
      <td>2022-09-05 07:00:00.000000 +00:00</td>
      <td>2022-10-14 16:00:00.000000 +00:00</td>
      <td>True</td>
      <td>False</td>
    </tr>
    <tr>
      <th>5</th>
      <td>ta_utTxMHwyldJ7neswh</td>
      <td>submitted</td>
      <td>13.20</td>
      <td>EUR</td>
      <td>temp</td>
      <td>7YBYgZ3OjoSMxgoXb</td>
      <td>2022-09-01 07:30:00.000000 +00:00</td>
      <td>2022-09-27 16:30:00.000000 +00:00</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>6</th>
      <td>ta_Y1EkWJKsIFmprF5CY</td>
      <td>submitted</td>
      <td>13.00</td>
      <td>EUR</td>
      <td>temp</td>
      <td>eynDaSFsB9GUhuDT2</td>
      <td>2022-08-31 08:00:00.000000 +00:00</td>
      <td>2022-10-19 17:00:00.000000 +00:00</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>7</th>
      <td>ta_y1xqKE4u44xdfE892</td>
      <td>submitted</td>
      <td>11.50</td>
      <td>EUR</td>
      <td>temp</td>
      <td>tPo2eHlpHLiVVsIMR</td>
      <td>2022-08-31 08:00:00.000000 +00:00</td>
      <td>2022-08-31 15:00:00.000000 +00:00</td>
      <td>False</td>
      <td>True</td>
    </tr>
    <tr>
      <th>8</th>
      <td>ta_CVWw8cjDFyW9N3TEX</td>
      <td>submitted</td>
      <td>13.00</td>
      <td>EUR</td>
      <td>temp</td>
      <td>1U0tZTNAhF6wZu26g</td>
      <td>2022-08-31 08:00:00.000000 +00:00</td>
      <td>2022-10-18 16:00:00.000000 +00:00</td>
      <td>True</td>
      <td>True</td>
    </tr>
    <tr>
      <th>9</th>
      <td>ta_ED9B9WHHTJhMP3kAv</td>
      <td>submitted</td>
      <td>13.00</td>
      <td>EUR</td>
      <td>temp</td>
      <td>1U0tZTNAhF6wZu26g</td>
      <td>2022-08-31 08:00:00.000000 +00:00</td>
      <td>2022-10-18 18:00:00.000000 +00:00</td>
      <td>False</td>
      <td>False</td>
    </tr>
  </tbody>
</table>
</div>




```python
df_tasks.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 1000 entries, 0 to 999
    Data columns (total 10 columns):
     #   Column                   Non-Null Count  Dtype  
    ---  ------                   --------------  -----  
     0   task_id                  1000 non-null   object 
     1   status                   1000 non-null   object 
     2   pricing_sider            1000 non-null   float64
     3   pricing_currency         1000 non-null   object 
     4   work_legal_status        1000 non-null   object 
     5   location_id              1000 non-null   object 
     6   start_date               1000 non-null   object 
     7   end_date                 1000 non-null   object 
     8   require_caces_5          1000 non-null   bool   
     9   require_driving_license  1000 non-null   bool   
    dtypes: bool(2), float64(1), object(7)
    memory usage: 64.6+ KB
    


```python
#Checking data type 
df_dict=df_tasks.location_id[df_tasks.location_id.apply(lambda x: isinstance(x, dict))]
df_list=df_tasks.location_id[df_tasks.location_id.apply(lambda x: isinstance(x, list))]
print(df_dict)
print(df_list)
```

    900    {'id': 'G5QE9Ox1xf8jEdmmG'}
    901    {'id': 'HwsHps3gsTfE9FgXl'}
    902    {'id': 'HwsHps3gsTfE9FgXl'}
    903    {'id': 'G5QE9Ox1xf8jEdmmG'}
    904    {'id': 'G5QE9Ox1xf8jEdmmG'}
                      ...             
    995    {'id': 'YE68ishRThcPadGjV'}
    996    {'id': 'aCXBCx6fY5t9DgXka'}
    997    {'id': 'TVf90cA0yYRhmSHok'}
    998    {'id': 'eArfj0NpHo9ilLAw7'}
    999    {'id': '54MDiK3LTcmGsG2TN'}
    Name: location_id, Length: 100, dtype: object
    800    [E9QlO33Voy4ii2Zh4]
    801    [6kg583OGRdtm5qHYm]
    802    [DKYDImXr1h6SlsoDP]
    803    [aJZXdV16eZdxAcdHa]
    804    [oIWkBhgnliciHM2ls]
                  ...         
    895    [Jr2OyBqWsJVW0yUgM]
    896    [G5QE9Ox1xf8jEdmmG]
    897    [G5QE9Ox1xf8jEdmmG]
    898    [G5QE9Ox1xf8jEdmmG]
    899    [iWIdn1jG02x0cjHOT]
    Name: location_id, Length: 100, dtype: object
    


```python
# Convert dict and list to string
df_tasks.location_id = df_tasks.location_id.apply(lambda x: x['id'] if isinstance(x,dict)  else x )
df_tasks.location_id = df_tasks.location_id.apply(lambda x: x[0] if isinstance(x,list) else x ) 
# Checking if there is some left
df_dict=df_tasks.location_id[df_tasks.location_id.apply(lambda x: isinstance(x, dict))]
df_list=df_tasks.location_id[df_tasks.location_id.apply(lambda x: isinstance(x, list))]
print(df_dict)
print(df_list)
```

    Series([], Name: location_id, dtype: object)
    Series([], Name: location_id, dtype: object)
    

I guessed that the centralized storage system must be in SQL because it is asked after in a the bonus question




```python
from sqlalchemy import create_engine
conn = create_engine('postgresql://postgres:mysecretpassword@localhost:5433/postgres')
df_sider_availabilities.to_sql('sider_availabilities', conn)
df_siders.to_sql("siders", conn)
df_task_locations.to_sql("task_locations", conn)
df_tasks.to_sql("tasks", conn)
```




    1000



### Here are the tables in the Sqlite Database

![alt text](tables.png "Title")

---
### Answer some analysis questions raised during the definition of the mission
1.  Compute and order by the city with the most siders first:<br>
    

a.  the total number of Siders per city


```python
SELECT city , count(*) nb_siders from siders group by city order by nb_siders desc 
```

b.  the total number of Siders per city that have CACES 5


```python
SELECT city , count(*) nb_siders from siders  
WHERE has_caces_5 = 1
group by city
order by nb_siders desc 

```

c. the percentage of Siders per city that have CACES 5


```python
Select a.city, round((a.nb_siders)/(b.nb_siders)*100,0) pc_sider_has_caces_5 
FROM 
(SELECT  city , (count(*)*1.0) nb_siders from siders  
WHERE has_caces_5 = 1
group by city
order by nb_siders desc) a
INNER join
(SELECT  city , (count(*)*1.0) nb_siders from siders  
group by city
order by nb_siders desc) b on a.city =b.city
```

## 2.  Compute the number of available Siders per task based on geographical 
location and on as-the-crows-flies (straight line) distance (Note: transport 
preference max distance indicate the maximum distance a Sider is ready to 
travel to go to work).


```python
--Version Sqlite3

SELECT count(1) nb_sider, 
t.task_id, 
round(SQRT(((a.latitude -b.latitude)*(a.latitude -b.latitude))+((a.longitude -b.longitude)*(a.longitude -b.longitude)))*100,2) distance_km 
FROM 
siders a , task_locations b 
INNER JOIN tasks t on t.location_id =b.location_id 
WHERE distance_km is not null and distance_km < a.transport_preference_max_distance 
GROUP BY t.task_id 
ORDER BY nb_sider
```


```python
--Verstion Postgis
--Necessité dans postgres de calculer en amont , ici la distance, afin de permettre de la filtrer avec une clause where
with  distance as (
    select round(ST_Distance(ST_MakePoint(s.latitude, s.longitude),ST_MakePoint(tl.latitude, tl.longitude))*100) as distance_km, s.sider_id , tl.location_id 
    FROM 
    siders s , task_locations tl 
 )
 
SELECT count(1) nb_sider, 
t.task_id, distance_km
from distance
INNER JOIN tasks t on t.location_id =distance.location_id 
INNER JOIN siders s  on s.sider_id =distance.sider_id 
WHERE distance_km is not null and distance_km < s.transport_preference_max_distance 
GROUP BY t.task_id , distance_km
ORDER BY nb_sider desc 

```

---
## 3.  BONUS: Write a SQL query that returns the number of Siders available per task 
based on all criteria that you find relevant?


```python
--Version Sqlite3

Select count(1) nb_sider, 
t.task_id, 
round(SQRT(((a.latitude -b.latitude)*(a.latitude -b.latitude))+((a.longitude -b.longitude)*(a.longitude -b.longitude)))*100,2) distance_km 
from 
siders a , task_locations b 
INNER JOIN tasks t on t.location_id =b.location_id 
INNER JOIN sider_availabilities sa on sa.sider_id =a.sider_id 
where distance_km is not null and distance_km < a.transport_preference_max_distance --if the location task is in the transport preference max distance of the sider
AND a.status='ready_to_work'  -- if the sider is ready to work
and a.blacklisted=FALSE        -- If the sider is not black listed
and (case WHEN t.require_caces_5= 1 THEN a.has_caces_5 = 1 else  true end ) -- if the sider has Caces 5 in case the task requiered own  
and (case WHEN t.require_driving_license = 1 THEN a.driving_license_validation_status  = 'Validated' else  true end ) -- If the sider has a driving licence if the task requiered own
AND (case WHEN strftime('%H',start_date )>'22' THEN sa.availability_type_options in ('full', 'night') else  true end ) -- If the sider can work nightly in case the mission is during night time
AND (case WHEN strftime('%H',start_date )<'22' THEN sa.availability_type_options ='day' else  true end )  -- If the sider can work daily incase the mission is during day time
AND (sa.availability_start_date < t.start_date  and  t.end_date < sa.availability_end_date) --if the sider is available diring task time
GROUP BY t.task_id 
order by nb_sider DESC  

```


```python
--version Postgis

with  distance as (
select round(ST_Distance(ST_MakePoint(s.latitude, s.longitude),ST_MakePoint(tl.latitude, tl.longitude))*100) as distance_km, s.sider_id , tl.location_id 
FROM 
siders s , task_locations tl 
 )
SELECT count(1) nb_sider, 
t.task_id, distance_km
from distance
INNER JOIN tasks t on t.location_id =distance.location_id 
INNER JOIN siders s  on s.sider_id =distance.sider_id 
INNER JOIN sider_availabilities sa on sa.sider_id =s.sider_id 
WHERE distance_km is not null and distance_km < s.transport_preference_max_distance 
AND s.status='ready_to_work'  -- if the sider is ready to work
and s.blacklisted=FALSE        -- If the sider is not black listed
and (case WHEN t.require_caces_5= true THEN s.has_caces_5 = 1 else true end ) -- if the sider has Caces 5 in case the task requiered own  
and (case WHEN t.require_driving_license = true THEN s.driving_license_validation_status  = 'Validated' else  true end ) -- If the sider has a driving licence if the task requiered own
AND (case WHEN date_part('hour', start_date::TIMESTAMP)>'22' THEN sa.availability_type_options in ('full', 'night') else  true end ) -- If the sider can work nightly in case the mission is during night time
AND (case WHEN date_part('hour', start_date::TIMESTAMP)<'22' THEN sa.availability_type_options ='day' else  true end )  -- If the sider can work daily incase the mission is during day time
AND (sa.availability_start_date < t.start_date  and  t.end_date < sa.availability_end_date) --if the sider is available diring task time
GROUP BY t.task_id , distance_km
ORDER BY nb_sider desc
```


      Input In [6]
        --version Postgis
                  ^
    SyntaxError: invalid syntax
    


---
## 4.  BONUS: Knowing that missions are worked only during business days, can you 
compute the Sider’s salary for doing the whole mission?


```python
-- Completely skraped from here : https://dba.stackexchange.com/questions/207701/count-business-days-between-2-dates-in-postgresql

with  distance as (
select round(ST_Distance(ST_MakePoint(s.latitude, s.longitude),ST_MakePoint(tl.latitude, tl.longitude))*100) as distance_km, s.sider_id , tl.location_id 
FROM 
siders s , task_locations tl 
 )
SELECT s.sider_id , 
t.task_id, distance_km

, count_business_days(
    TO_DATE(t.start_date, 'YYYY-MM-DD'),
    TO_DATE(t.end_date, 'YYYY-MM-DD')
    ) 
	* t.pricing_sider 
	* (extract(hour from  end_date::timestamp) - extract(hour from  start_date::timestamp)) as salary
from distance
INNER JOIN tasks t on t.location_id =distance.location_id 
INNER JOIN siders s  on s.sider_id =distance.sider_id 
INNER JOIN sider_availabilities sa on sa.sider_id =s.sider_id 
WHERE distance_km is not null and distance_km < s.transport_preference_max_distance 
AND s.status='ready_to_work'  -- if the sider is ready to work
and s.blacklisted=FALSE        -- If the sider is not black listed
and (case WHEN t.require_caces_5= true THEN s.has_caces_5 = 1 else true end ) -- if the sider has Caces 5 in case the task requiered own  
and (case WHEN t.require_driving_license = true THEN s.driving_license_validation_status  = 'Validated' else  true end ) -- If the sider has a driving licence if the task requiered own
AND (case WHEN date_part('hour', start_date::TIMESTAMP)>'22' THEN sa.availability_type_options in ('full', 'night') else  true end ) -- If the sider can work nightly in case the mission is during night time
AND (case WHEN date_part('hour', start_date::TIMESTAMP)<'22' THEN sa.availability_type_options ='day' else  true end )  -- If the sider can work daily incase the mission is during day time
AND (sa.availability_start_date < t.start_date  and  t.end_date < sa.availability_end_date) --if the sider is available diring task time 
GROUP BY t.task_id , distance_km , t.start_date , t.end_date , t.pricing_sider , s.sider_id
ORDER BY  salary desc 
```
