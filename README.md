# Calculate the total number of contacts for each channel within each customer segment
total_contacts_per_channel_per_segment = events_df.groupby(['segment', 'channel']).size().reset_index(name='total_contacts')

# Display the data
total_contacts_per_channel_per_segment.head(10)
