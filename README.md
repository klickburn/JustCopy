# Calculate the total number of contacts within each account balance range
total_contacts_per_balance_range = events_df.groupby('balance_range').size().reset_index(name='total_contacts')

# Merge the total_payments_per_balance_range and total_contacts_per_balance_range DataFrames
ratio_balance_range_df = pd.merge(total_payments_per_balance_range, total_contacts_per_balance_range, on='balance_range')

# Calculate the ratio of payments to contacts for each account balance range
ratio_balance_range_df['payment_to_contact_ratio'] = ratio_balance_range_df['total_payments'] / ratio_balance_range_df['total_contacts']

# Display the data
ratio_balance_range_df
