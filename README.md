# Step 1: Identify the first channel of contact and its date for each account
first_contact = events_df.groupby('acct_ref_nb').first().reset_index()

# Merge with the main dataframe to get future events for these accounts
merged_data = pd.merge(first_contact, events_df, on='acct_ref_nb', suffixes=('_first', '_event'))

# Filter where the event date is within 30 days of the first contact date and the event is 'PYMT'
payment_within_30_days = merged_data[
    (merged_data['date_event'] > merged_data['date_first']) &
    (merged_data['date_event'] <= merged_data['date_first'] + timedelta(days=30)) &
    (merged_data['channel_event'] == 'PYMT')
]

# Count unique accounts that made a payment within 30 days grouped by the first contact channel
accounts_paid = payment_within_30_days.groupby('channel_first')['acct_ref_nb'].nunique()

# Count unique accounts contacted via each channel
total_accounts_contacted = first_contact['channel'].value_counts()

# Calculate the percentage
percentage_paid = (accounts_paid / total_accounts_contacted) * 100

percentage_paid.fillna(0).to_dict()
