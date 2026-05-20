# Sports-Performance-Analysis

# Load the packages

import pandas as pd
import numpy as np
import statsmodels.formula.api as smf
import matplotlib.pyplot as plt
import seaborn as sns

# Load the data. 
# EPL results for the 2017/18 season

EPL18 = pd.read_excel('Assignment Data/Week 1/EPL2017-18.xlsx')
print(EPL18.columns.tolist())
EPL18

# Assigning values for wins, draws & losses

EPL18['hwinvalue']=np.where(EPL18['FTR']=='H',1,np.where(EPL18['FTR']=='D',0.5,0))
EPL18['awinvalue']=np.where(EPL18['FTR']=='A',1,np.where(EPL18['FTR']=='D',0.5,0))
EPL18['count']=1
EPL18

# Breaking the data into two parts

EPL2017 = EPL18[EPL18.Date<20180000]
EPL2018 = EPL18[EPL18.Date>20180000]
EPL2017
EPL2018

# Home team summary for 2017

EPL2017h = EPL2017.groupby('HomeTeam')['FTHG','FTAG','hwinvalue','count'].sum().reset_index()
EPL2017h = EPL2017h.rename(columns={'HomeTeam':'Team','FTHG':'FTHGh17','FTAG':'FTAGh17','hwinvalue':'hwinvalue17','count':'Gh17'})
EPL2017h

# Away team summary for 2017

EPL2017a = EPL2017.groupby('AwayTeam')['FTHG','FTAG','awinvalue','count'].sum().reset_index()
EPL2017a = EPL2017a.rename(columns={'AwayTeam':'Team','FTHG':'FTHGa17','FTAG':'FTAGa17','awinvalue':'awinvalue17','count':'Ga17'})
EPL2017a

# Merging both

EPL2017 = pd.merge(EPL2017h,EPL2017a,on='Team')
EPL2017

# Calculating values 2017

EPL2017['W17'] = EPL2017['hwinvalue17'] + EPL2017['awinvalue17']
EPL2017['G17'] = EPL2017['Gh17'] + EPL2017['Ga17']
EPL2017['GF17'] = EPL2017['FTHGh17'] + EPL2017['FTAGa17']
EPL2017['GA17'] = EPL2017['FTAGh17'] + EPL2017['FTHGa17']
EPL2017

# Win Percentage & Pyth. Exp. 2017

EPL2017['wpc17'] = EPL2017['W17']/EPL2017['G17']
EPL2017['pyth17'] = EPL2017['GF17']**2/(EPL2017['GF17']**2 + EPL2017['GA17']**2)
EPL2017['diff'] = EPL2017['wpc17'] - EPL2017['pyth17']
EPL2017

# Home team summary for 2018

EPL2018h = EPL2018.groupby('HomeTeam')['FTHG','FTAG','hwinvalue','count'].sum().reset_index()
EPL2018h = EPL2018h.rename(columns={'HomeTeam':'Team','FTHG':'FTHGh18','FTAG':'FTAGh18','hwinvalue':'hwinvalue18','count':'Gh18'})
EPL2018h

# Away team summary for 2018

EPL2018a = EPL2018.groupby('AwayTeam')['FTHG','FTAG','awinvalue','count'].sum().reset_index()
EPL2018a = EPL2018a.rename(columns={'AwayTeam':'Team','FTHG':'FTHGa18','FTAG':'FTAGa18','awinvalue':'awinvalue18','count':'Ga18'})
EPL2018a

# Merging both

EPL2018 = pd.merge(EPL2018h,EPL2018a,on='Team')
EPL2018

# Calculating values 2018

EPL2018['W18'] = EPL2018['hwinvalue18'] + EPL2018['awinvalue18']
EPL2018['G18'] = EPL2018['Gh18'] + EPL2018['Ga18']
EPL2018['GF18'] = EPL2018['FTHGh18'] + EPL2018['FTAGa18']
EPL2018['GA18'] = EPL2018['FTAGh18'] + EPL2018['FTHGa18']
EPL2018

# Win Percentage & Pyth. Exp. 2018

EPL2018['wpc18'] = EPL2018['W18']/EPL2018['G18']
EPL2018['pyth18'] = EPL2018['GF18']**2/(EPL2018['GF18']**2 + EPL2018['GA18']**2)
EPL2018['diff'] = EPL2018['wpc18'] - EPL2018['pyth18']
EPL2018['diffvalue'] = EPL2018['hwinvalue18'] - EPL2018['awinvalue18']
EPL2018

# Merging 2017 & 2018 summaries

Merge = pd.merge(EPL2017,EPL2018,on='Team')
Merge

# Correlation Analysis

keyvars = Merge[['Team','wpc18','wpc17','pyth17','pyth18']]
keyvars.corr()
