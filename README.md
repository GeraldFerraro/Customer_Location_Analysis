# Customer_Location_Analysis
# Gerald Ferraro
# 8/15/2023

'''
Locational Analysis of Customer Data
The purpose of the project is to work with customer data taken from liability waivers at my axe throwing business. We have teamed up with a marketing firm in order to grow our customer base. For the marketing firm to allocate capital effectively, they need to better understand where the majority of our customers are coming from. 

I used Jupyter Notebook to develop a notebook where I can showcase the data exploration, cleaning, processing, and analysis. After removing out of state customers and other irrelevant entries, I took 16,000+ unique entries down to 14,000. Our location can be a destination. However eighty-four percent of our customers come from just twenty one of the one hundred and sixty nine towns in Connecticut.

Next I built multiple different visualizations on Tableau and built a report explaning to my stake holders some further background on my processes and then communicated my findings.
'''


Spatial Analysis of Customer Profiles

import pandas as pd
import pandas as pd
import matplotlib.pyplot as plt
​
bpt_cust = pd.read_csv('C:\\Users\\geral\\Documents\\LucidCustomerInfo\\BptCustomerProfiles.csv')
bptcust

#Start Exploring the Data

bpt_cust.columns
#We only want to analyze location and age for our project so we will narrow the columns down to relevant data.

#Create a subset of our larger data set.
subset_bpt_cust = bpt_cust[['City', 'State', 'Date of Birth']]
subset_bpt_cust
subset_bpt_cust.isna().sum()

#Explore some basic statistics about the data set.
subset_bpt_cust.describe()
subset_bpt_cust.dtypes
cities_freq = subset_bpt_cust['City'].value_counts()
cities_freq
states_freq = subset_bpt_cust['State'].value_counts()
states_freq
pd.DataFrame(cities_freq).plot(kind = 'bar', figsize = (20,10))
pd.DataFrame(states_freq).plot(kind = 'bar', figsize = (20,10))
After exploration we can now go through and clean up the issues we noticed during our exploration process.

#Drop the two na values in the location based data
location_bpt_cust = subset_bpt_cust[['City', 'State']]
location_bpt_cust = location_bpt_cust.dropna()
location_bpt_cust
#check all na values have been removed
location_bpt_cust.isna().sum()

#Fix some common mispellings
location_bpt_cust['State'] = location_bpt_cust['State'].replace(['Ct', 'Connecticut', 'Connecticut ', 'ConnectuIcut', 'Connecituct', 'Ct. Connecticut ','CONNECTICUT'],'CT')
location_bpt_cust['State'].value_counts()
location_bpt_cust['State'].unique().tolist()

#Create a states to drop value for all of the items in the list above that do not fit the parameters of our analysis
states_to_drop = ['MD', 'VA', 'NY', 'NEW YORK', 'Ny', 'New York', 'New yoirk', 'New york', 'NJ', 'VT', 'New Jersey', 'MA',
                  'FLORIDA', 'New Jersey ', 'Ctah', 'California', 'Il', 'New York ', 'Pennsylvania ', 'Nj', 'Pa ', 'Co', 'Me',
                  'IN', 'Nj ', 'Massachusetts ', 'NH', 'Pa', 'Rhode Island ', 'TX', 'New jersey', 'NC', 'GA', 'Trumbull ', 
                  'Florida', 'Vermont', 'Vt', 'Pennsylvania', 'Rhode Island', 'ME', 'new york', 'CA', 'FL', 'new jersey',
                  'Massachusetts', 'RI', 'Illinois ', 'CR', 'NY - New YorkNY', 'New Hampshire ', 'Ma', 'ma', 'Maine',
                  'Maryland ', 'South carolina', 'SC', 'MN', 'Default', 'Alaska', 'Tennessee ', 'Ny ', 'KY', 'Zuid Holland',
                  'Fl', 'PA', 'Rhode Islsnd', 'Georgia', 'NY ', 'Germany ', 'MI', 'Delaware', 'United States (1)', 'Alabama ',
                  'ID', 'ny', 'NRW', 'Michigan', 'Germany', 'OH', 'New Hampshire', 'ON', 'IL', 'Balls', 'Ohio', 'Virginia',
                  'Noord Holland', 'il', 'Netherlands', 'Wisconsin', 'São Paulo', 'Illinois', 'SP', 'california', 'US',
                  'Virginia ', 'Ma ', 'Colorado', 'CO', 'MAINE', 'NJ ', 'ny ', 'Massachesetts', 'WI', 'WV', 'new york ',
                  'new York', 'c', 'OR', 'new Jersey', 'United States', 'Delaware ', 'DE', 'Ri', 'State', ' New York ',
                  'or', 'Florida ', 'FloridA', 'Confusion', 'DC', 'fl', 'va', 'North Carolina', 'ri', 'NV', 'CA ', 'Massachusettss',
                  'USA', 'Oregon', 'state', 'Tx', ' NEw york', 'New hampshire', 'Nh', 'North carolina', 'Washington ', 'Mi',
                  'Michigan ', 'Nc', 'Ca', 'Capital Federal', 'California ', '6824', 'Missouri', 'Wi', 'Mass', 'Rhode island',
                  '310 webb circle', 'Md', 'Washington', 'Alberta', 'Kansas', 'Maryland', 'Dc', 'Va', 'Hawaii', 'Minnesota ',
                  'Ohio ', 'Nevada', 'Ga', 'Richmond Hill', 'Texas', 'New hampsire', 'PA ', 'Tennessee', 'Nv ', 'Texas ', 'Minnesota',
                  'New ayork', 'Md ', 'Wolfpit', ' ', 'Ill', 'NU', 'In', 'Colombia', 'Indiana', 'New Jeresy', 'Oklahoma ',
                  'South Carolina ', 'on', 'Puerto Rico', 'Sc', 'Denmark', 'Oregon ', 'Mt', 'N/a', 'North Carolina ', 'Ok', 'Nv',
                  '777 Wheelers Rd', 'Apt 304', 'TN', '', 'Oklahoma', 'South Carolina', 'Alabama']
​
​#Replace the rest of the unique misspellings and missinformation replacing all those that apply with the uniform label "CT".
location_bpt_cust['State'] = location_bpt_cust['State'].str.strip()
location_bpt_cust = location_bpt_cust[~location_bpt_cust['State'].isin(states_to_drop)]
print(location_bpt_cust)
location_bpt_cust['State'].unique().tolist()
location_bpt_cust['State'] = location_bpt_cust['State'].str.strip().str.upper()
location_bpt_cust['State'].value_counts()
location_bpt_cust['State'].unique().tolist()
location_bpt_cust['State'] = location_bpt_cust['State'].replace([ 'CAT',
 'CT - CONNECTICUT', 'CONNCTICUT', 'CONNETICUT', 'CONNECTICUT', 'CONNETICUIT',
 'TRUMBULL', 'CONNECTIUCT', 'COMMECTICUT', 'DANBURY,CT', 'CTT', 'CONNECTICUT (CT)',
 'CFT', 'CONNECTICUT-CT', 'CT.', 'CONNECTUCUT', 'CR', 'CONN', 'CY', 'CONETICUT',
 'CONNECITCUT', 'CONNECTION', 'CT06811', 'CT.US', 'BRIDGEPORT', 'CONMECTICUT',
 '[CT] CONNECTICUT', 'STAMFORD', 'CT-CONNECTICUT', 'FAIRFIELD', 'CONNECTIUT',
 'CONNECTICUTT'],'CT')
 
location_bpt_cust['State'].value_counts()

#Append the list variable to include the value we previously missed
states_to_drop.append('NEW YORK')
location_bpt_cust = location_bpt_cust[~location_bpt_cust['State'].isin(states_to_drop)]

location_bpt_cust['State'].unique().tolist()
print(location_bpt_cust)

location_bpt_cust['City'].unique().tolist()
location_bpt_cust['City'].value_counts()
location_bpt_cust['City'] = location_bpt_cust['City'].str.strip().str.upper()
location_bpt_cust['City'].value_counts()
location_bpt_cust['City'].unique().tolist()

#Fix common spelling errors for the more common towns in the data set
location_bpt_cust['City'] = location_bpt_cust['City'].replace(['FAIRFIE;D','FAIRIFELD','FAIRFILED','FAIFIELD', 'FFLD',
                                                               'FAIRFELD'], "FAIRFIELD")
location_bpt_cust['City'] = location_bpt_cust['City'].replace(['BLACK ROCK', 'BPT', '244 VINCELLETTE ST BRIDGEPORT', 'BOPO',
                                                               '244 VINCELLETTE ST BRIDGEPORT', 'BRIGEPORT', 'BRIDEPORT', 
                                                               '35 GARDEN TERRACE BRIDGEPORT', 'BRIDGERPORT', '6606'], "BRIDGEPORT")
location_bpt_cust['City'] = location_bpt_cust['City'].replace(['GREENWICH, CT','GREEWNICH','GTEENWICH'], "GREENWICH")
location_bpt_cust['City'] = location_bpt_cust['City'].replace(['STRAFORD','STRATFOTD', 'STFD', 'STRATFIRD','STRATRFORD', 
                                                               'STRATFORF', 'STFD', 'DTRATFORD',], "STRATFORD")
location_bpt_cust['City'] = location_bpt_cust['City'].replace(['STAM', 'STAMFORD, CT', 'STAMFIRD', 'STANFORD'], "STAMFORD")
location_bpt_cust['City'] = location_bpt_cust['City'].replace(['MILFY','21 AVON ST MILFORD CT', 'MILFOD', 'MILLFORD', 
                                                               'MILFORD, CT', 'MIKFORD', '92 PLAINS ROAD UNIT C72',], "MILFORD")
location_bpt_cust['City'] = location_bpt_cust['City'].replace(['24 LANCASTER DR NORWALK', 'NOTWAL', 'NORWALK, CT', 'NORWAK', 
                                                               'NOTWALK', 'NORWALK HAS', 'NORWLK'], "NORWALK")
location_bpt_cust['City'] = location_bpt_cust['City'].replace(['TRUMBULK','TRUMBULLL', 'TRUMNULL', 'TRUMBOLL', 'TRUMBUL',
                                                               'TRUMBUKK', '6611'], "TRUMBULL")
location_bpt_cust['City'] = location_bpt_cust['City'].replace(['NO HAVEN', ], "NORTH HAVEN") 
location_bpt_cust['City'] = location_bpt_cust['City'].replace(['MIDDELTOWN', ], "MIDDLETOWN") 
location_bpt_cust['City'] = location_bpt_cust['City'].replace(['CHESIRE'], "CHESHIRE")
location_bpt_cust['City'] = location_bpt_cust['City'].replace(['WTERBURY'], "WATERBURY")
location_bpt_cust['City'] = location_bpt_cust['City'].replace(['WILTON, CT'], "WILTON")
location_bpt_cust['City'] = location_bpt_cust['City'].replace(['WESTON, CT', 'WESTON CT'], "WESTON")
location_bpt_cust['City'] = location_bpt_cust['City'].replace(['WALCOTT'], "WOLCOTT")
location_bpt_cust['City'] = location_bpt_cust['City'].replace(['WASTE HAVEN','6516'], "WEST HAVEN")
location_bpt_cust['City'] = location_bpt_cust['City'].replace(['RIDGE FIELD', 'RIDGEFILED'], "RIDGEFIELD")
location_bpt_cust['City'] = location_bpt_cust['City'].replace(['NEWCANAAN', 'NEW CANNON'], "NEW CANAAN")
location_bpt_cust['City'] = location_bpt_cust['City'].replace(['WESTPOT'], "WESTPORT")
location_bpt_cust['City'] = location_bpt_cust['City'].replace(['STORRS MANSFIELD'], "STORRS")
location_bpt_cust['City'] = location_bpt_cust['City'].replace(['SOUTHBURY, CONNECTICUT'], "SOUTHBURY")
location_bpt_cust['City'] = location_bpt_cust['City'].replace(['MONRORE','MONRORE', 'MINROE'], "MONROE")
location_bpt_cust['City'] = location_bpt_cust['City'].replace(['JEWETT  CITY'], "JEWETT CITY")
location_bpt_cust['City'] = location_bpt_cust['City'].replace(['6401'], "ANSONIA")
location_bpt_cust['City'] = location_bpt_cust['City'].replace(['6484', 'SHETON', 'SHWLTONT'], "SHELTON")
​
​
location_bpt_cust['City'].value_counts()
cities = location_bpt_cust['City'].unique().tolist()
cities.sort(reverse=True)
cities

#Take all of the address errors. Since there are not that many of them we will drop them. 
location_bpt_cust['City'].value_counts(normalize=True)
address_drop = [ '6 BORGLUM STREE', '59 HANFORD', '55 HOLLAND ROAD', '52 KING ST', '50 GREENVIEW LANE', '5 GLASTONBURY PLACE',
                '5 BUTLER LANE', '46 GARDEN DRIVE', '41 CANDLEWOOD RD', '38 HINE STREET', '32 COS COB AV', '30 BLUE HILLS ROAD',
                '3 N BRIDGE ST', '289 LITCHFIELD TPKE', '24  MITCHELL RD', '23 ALLAN STREET', '165 FILLOW ST', '163 HICKORY LANE',
                '15 RYAN AVE', '12 NASH PL', '106 REITTER ST', '1 HIAWATHA TRAIL' '80 GILL ST', '74 HOWARD AVE 2ND FLOOR',
                '1 HIAWATHA TRAIL',  '80 GILL ST']
location_bpt_cust = location_bpt_cust[~location_bpt_cust['City'].isin(address_drop)]
location_bpt_cust['City'].value_counts().sort_values()

#Create a variable to show us the top 21 towns that hold the vast majority of our customer base.
freq_city = location_bpt_cust['City'].value_counts().index[0:21]
freq_city = freq_city.tolist()
freq_city
core_customers = location_bpt_cust[location_bpt_cust['City'].isin(freq_city)]
core_customers.value_counts(normalize=True)
core_customers.value_counts()
#Calculate how much data we have lost by only including the top 21 towns
​
core_customers.value_counts().sum()/len(location_bpt_cust)
About 84% of our customer base comes from 21 towns and 16% of our customer base comes from over 140 towns in Connecticut.

The goal of this project has been to identify where the majority of our customers are coming from locally. This will allow our marketing team to economize our marketing efforts as efficiently as possible.

freq = core_customers['City'].value_counts()

#Create a bar chart to show us the visual breakdown of our customer city frequencies.
freq.plot(kind='bar', title = 'Frequency of City in Customer Profiles', xlabel = 'City', ylabel = 'Count', figsize=(10,8))
plt.show()
​
​#Create a pie chart to show us the visual breakdown of our distribution.
freq.plot(kind='pie', title = 'Frequency of City in Customer Profiles', figsize=(20,10), autopct='%.0f%%')
plt.show()
#Save the data frame as a .csv file for future visualization work in tableau

core_customers.to_csv("C:\\Users\\geral\\Documents\\LucidCustomerInfo" + "core_customers.csv", index = False)
#Save the data frame as a .csv file for future visualization work in tableau
​
