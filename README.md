# Function to count the total population in each bucket for each month
def count_population_in_buckets(df):
    return df['bucket'].value_counts().to_dict()

# Dictionary to store the total population in each bucket for each month
population_in_buckets_each_month = {}

# Loop through the dataframes and calculate the population in buckets
for df in monthly_dfs:
    month_name = df.columns.name
    population_in_buckets_each_month[month_name] = count_population_in_buckets(df)

population_in_buckets_each_month

