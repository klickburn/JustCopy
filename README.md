# Now, let's proceed with calculating Insight 2: Re-engagement Channels

# Sort the events_df by account and date for tracking customer activity
sorted_events_df = events_df.sort_values(by=['acct_ref_nb', 'date'])

# Create a new column to hold the date of the previous contact for each account
sorted_events_df['prev_contact_date'] = sorted_events_df.groupby('acct_ref_nb')['date'].shift(1)

# Calculate the days since the last contact for each account
sorted_events_df['days_since_last_contact'] = (sorted_events_df['date'] - sorted_events_df['prev_contact_date']).dt.days

# Define "inactive" as not having any contact for at least 30 days and create a flag for these records
sorted_events_df['is_reengagement'] = sorted_events_df['days_since_last_contact'] >= 30

# Filter the DataFrame to only include re-engagement records
reengagement_df = sorted_events_df[sorted_events_df['is_reengagement']]

# Count the number of re-engagements for each channel
reengagement_count_by_channel = reengagement_df['channel'].value_counts().reset_index()
reengagement_count_by_channel.columns = ['channel', 'reengagement_count']

# Display the re-engagement count by channel
reengagement_count_by_channel
