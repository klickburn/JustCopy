# Merging the first_channel_df with the original events_df to get all events for these accounts
merged_df = pd.merge(events_df, first_channel_df[['acct_ref_nb', 'date', 'channel']], 
                     on='acct_ref_nb', suffixes=('', '_first'))

# Filtering for events that are 'PYMT' and occurred within 30 days of the first channel contact
pymt_within_30_days = merged_df[
    (merged_df['channel'] == 'PYMT') & 
    (merged_df['date'] <= merged_df['date_first'] + timedelta(days=30))
]

# Count of accounts that made a payment within 30 days of the first channel contact
pymt_count = pymt_within_30_days['acct_ref_nb'].nunique()

# Calculating the percentage
percentage = (pymt_count / first_channel_df['acct_ref_nb'].nunique()) * 100

pymt_count, percentage

# Finding accounts that made a payment within 30 days for each first channel
pymt_counts_by_channel = pymt_within_30_days.groupby('channel_first')['acct_ref_nb'].nunique()

# Calculating the percentage for each channel
percentage_by_channel = (pymt_counts_by_channel / first_channel_count) * 100

# Combining the counts and percentages into a DataFrame for better presentation
channel_stats_df = pd.DataFrame({
    'Accounts_with_First_Contact': first_channel_count,
    'Payments_within_30_days': pymt_counts_by_channel,
    'Percentage': percentage_by_channel
}).fillna(0)  # Replace NaN with 0

channel_stats_df
