# Identify the most common customer segment within each account balance range
most_common_segment_per_balance_range = events_df.groupby(['balance_range', 'segment']).size().reset_index(name='frequency')
most_common_segment_per_balance_range = most_common_segment_per_balance_range.sort_values(['balance_range', 'frequency'], ascending=[True, False])
most_common_segment_per_balance_range = most_common_segment_per_balance_range.drop_duplicates('balance_range')

# Display the data
most_common_segment_per_balance_range
