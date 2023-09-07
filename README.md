# Calculate the total number of contacts for each channel within each bucket
total_contacts_per_channel_per_bucket = events_df.groupby(['bucket', 'channel']).size().reset_index(name='total_contacts')

# Display the data
total_contacts_per_channel_per_bucket.head(10)
