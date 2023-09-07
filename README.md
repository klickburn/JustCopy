# Identify the most common customer segment within each bucket
most_common_segment_per_bucket = events_df.groupby(['bucket', 'segment']).size().reset_index(name='frequency')
most_common_segment_per_bucket = most_common_segment_per_bucket.sort_values(['bucket', 'frequency'], ascending=[True, False])
most_common_segment_per_bucket = most_common_segment_per_bucket.drop_duplicates('bucket')

# Display the data
most_common_segment_per_bucket
