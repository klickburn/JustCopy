# Calculate the total number of contacts within each customer segment
total_contacts_per_segment = events_df.groupby('segment').size().reset_index(name='total_contacts')

# Merge the total_payments_per_segment and total_contacts_per_segment DataFrames
ratio_segment_df = pd.merge(total_payments_per_segment, total_contacts_per_segment, on='segment')

# Calculate the ratio of payments to contacts for each customer segment
ratio_segment_df['payment_to_contact_ratio'] = ratio_segment_df['total_payments'] / ratio_segment_df['total_contacts']

# Display the data
ratio_segment_df
