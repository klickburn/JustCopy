# Grouping by account and channel and counting occurrences
channel_counts = events_df.groupby(['acct_ref_nb', 'channel']).size().reset_index(name='counts')

# Calculating the average for each channel across all accounts
average_counts = channel_counts.groupby('channel')['counts'].mean()

average_counts
