# Monetary Data Analysis

This project performs a detailed analysis of various monetary data including GDP, money supply (M1, M2, M3), and currency velocity (M1V, M2V). It fetches, processes, and visualizes these data sets to explore trends and relationships over time.

## Prerequisites

### Required Libraries

Ensure you have the following libraries installed:

- `pandas`
- `matplotlib`
- `seaborn`
- `numpy`
- `tqdm`

You can install these using pip:

```sh
pip install pandas matplotlib seaborn numpy tqdm
```

### Data Files

You will need the following CSV files:

- `GDP.csv`
- `M1US.csv`
- `M1V.csv`
- `M2US.csv`
- `M2V.csv`
- `M3US.csv`
- `Marketyeld1.csv`

Place these files in the same directory as the script.

## Script Overview

The script performs the following main tasks:

1. **Load and Clean Data**: Reads the data from the CSV files and converts date columns to datetime objects.
2. **Data Interpolation**: Fills in missing data by interpolating values between dates.
3. **Merge Data**: Merges the various datasets into a single dataframe based on the date.
4. **Data Visualization**: Plots several graphs to visualize the trends and relationships in the data.

### Functions

- `to_datetime(df)`: Converts the `DATE` column of a dataframe to datetime objects.
- `filler(date_col, entity_col, entity)`: Interpolates values between dates for a given entity.
- Main script processes the data and generates visualizations using seaborn and matplotlib.

## Data Processing

The script reads each CSV file into a pandas dataframe and converts the date columns to datetime objects using the `to_datetime` function.

```python
df_GDP = to_datetime(df_GDP)
df_M1US = to_datetime(df_M1US)
df_M1V = to_datetime(df_M1V)
df_M2US = to_datetime(df_M2US)
df_M2V = to_datetime(df_M2V)
df_M3US = to_datetime(df_M3US)
df_M1Y = to_datetime(df_M1Y)
```

### Data Interpolation

The `filler` function fills in missing data points by interpolating between known values. This is done for each dataset.

```python
df_dict = {}
for key, value in df_dicts.items():
    df = df_dicts[key]
    if key == 'df_M1Y':
        df.drop(df.loc[df['DGS1'] == '.'].index, inplace=True)
        df_M1Y['DGS1'] = df_M1Y['DGS1'].astype('float')
    df_dict[key] = filler(df[df.columns[0]], df[df.columns[1]], key)
```

### Merge Data

The dataframes are merged into a single dataframe based on the date column.

```python
for key, value in df_dict.items():
    if key == 'df_GDP':
        big_df = df_dict[key]
    else:
        dfs = df_dict[key]
        big_df = pd.merge(big_df, dfs, on="date")
big_df.drop_duplicates(subset=['date'], inplace=True)
```

### Visualization

The script generates several visualizations to explore the trends and relationships in the data:

1. **Monetary Supply in Billions**: Compares M1 and M2 monetary supplies over time.
2. **Currency Velocity**: Compares the velocity of M1 and M2 over time.
3. **Monetary Velocity in Relation to Monetary Supply**: Compares the monetary velocity of M1 and M2, normalized to 1981 levels.
4. **M1 Velocity in Relation to M2**: Compares M1 velocity to M2 velocity.
5. **Monetary Velocity in Relation to Monetary Supply (M2 normalized)**: Another view of monetary velocity, focusing on M2.
6. **Currency Velocity (Scaled)**: Compares the velocity of M1, M2, and M1 yield, normalized to 1981 levels.

Each plot is created using seaborn and matplotlib:

```python
a = plt.figure(figsize=(20, 10))
a = sns.lineplot(big_df['date'], big_df['M1US'])
a = sns.lineplot(big_df['date'], big_df['M2US'])
a.xaxis.set_major_locator(mdates.YearLocator(base=2))
a.xaxis.set_major_formatter(mdates.DateFormatter('%Y'))
a.yaxis.set_ticks(np.arange(0, 25000, 2500))
plt.title('Monetary Supply in Billions')
plt.grid()
plt.show()
```

## Conclusion

This project provides a comprehensive analysis of monetary data, illustrating trends and relationships through various visualizations. The script is flexible and can be extended to include additional datasets or more sophisticated analysis techniques.
