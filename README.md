# Calculate the total number of contacts within each bucket
total_contacts_per_bucket = events_df.groupby('bucket').size().reset_index(name='total_contacts')

# Merge the total_payments_per_bucket and total_contacts_per_bucket DataFrames
ratio_df = pd.merge(total_payments_per_bucket, total_contacts_per_bucket, on='bucket')

# Calculate the ratio of payments to contacts for each bucket
ratio_df['payment_to_contact_ratio'] = ratio_df['total_payments'] / ratio_df['total_contacts']

# Display the data
ratio_df
