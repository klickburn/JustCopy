# Filter the dataframe to get accounts with EMAIL as the first contact
first_contact_email = sorted_events_df.drop_duplicates(subset=['acct_ref_nb'], keep='first')
first_contact_email = first_contact_email[first_contact_email['channel'] == 'EMAIL']

# Filter the dataframe to get accounts that made a payment
payments = sorted_events_df[sorted_events_df['channel'] == 'PYMT']

# Merge the two dataframes to get accounts with EMAIL as the first contact and made a payment
merged_df = pd.merge(first_contact_email, payments, on='acct_ref_nb', suffixes=('_email', '_pymt'))

# Filter the merged dataframe to get accounts that made a payment within 30 days of the first contact
accounts_matching_criteria = merged_df[merged_df['date_pymt'] - merged_df['date_email'] <= pd.Timedelta(days=30)]

# Extract the unique account reference numbers
unique_accounts = accounts_matching_criteria['acct_ref_nb'].unique()

unique_accounts
