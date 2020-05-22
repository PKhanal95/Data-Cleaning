# Data-Cleaning

Estimates suggest that data scientists spend around 60-80% of their time cleaning and preparing data. This is not an over-estimation! Your project data is rarely ready to be loaded straight into your machine learning models.
There may be numerous challenges, such as:
Missing data
Poorly formatted data 
Misspellings
Duplicates
Errors
Outliers
Dealing with these sorts of challenges is called data cleansing. 

Converting data types
df['number_of_bedrooms'] = pd.to_numeric(df['number_of_bedrooms'])
df['price'] = pd.to_numeric(df['price'])

replacing values
non_nums = df[~df["number_of_bedrooms"].str.isnumeric()]["number_of_bedrooms"].unique()
df["number_of_bedrooms"] = df["number_of_bedrooms"].replace(non_nums, np.nan)
df["type"] = df["type"].replace(['teraced'], 'terraced')
df["price"] = df["price"].apply(lambda x: x.replace('£', '') if type(x) is str else x)
df["price"] = df["price"].apply(lambda x: x.replace(',', '') if type(x) is str else x)

removing outliers
df = df.drop(7)
df.loc[7,'number_of_bedrooms'] = np.NaN
df.groupby('location')['number_of_bedrooms'].transform(lambda x: x.fillna(x.mean()))
df = df.drop_duplicates()

removing duplicates
mean = df["price"].mean()    
df["price"] = df["price"].fillna(value=mean) 

imputing nulls with mean and medians using python libraries
# Extract the year
df['year'] = df.date_of_sale.dt.year

# Impute null prices with the mean for the location, bedrooms and year of sale
df.groupby(['location','number_of_bedrooms','year'])['price'].transform(lambda x: x.fillna(x.mean()))
