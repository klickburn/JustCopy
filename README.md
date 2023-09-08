# First, we need to create a column that represents the date of the next event for each account
sorted_events_df['next_event_date'] = sorted_events_df.groupby('acct_ref_nb')['date'].shift(-1)

# Filter to get the first contact for each account
first_contact_df = sorted_events_df.drop_duplicates(subset=['acct_ref_nb'], keep='first')

# Filter for payments within 30 days of the first contact
payments_within_30_df = sorted_events_df[(sorted_events_df['channel'] == 'PYMT') & 
                                         (sorted_events_df['date'] - sorted_events_df['next_event_date'] <= pd.Timedelta(days=30))]

# Merge the two dataframes on account number
merged_df = pd.merge(first_contact_df, payments_within_30_df, on='acct_ref_nb', how='left', suffixes=('_first', '_payment'))

# Check if payment was made within 30 days for each first contact
merged_df['payment_within_30_days'] = ~merged_df['date_payment'].isna()

# Calculate the percentage of accounts making a payment within 30 days for each first contact channel
payment_percentages = merged_df.groupby('channel_first')['payment_within_30_days'].mean().reset_index()
payment_percentages['percentage'] = payment_percentages['payment_within_30_days'] * 100

payment_percentages[['channel_first', 'percentage']]
