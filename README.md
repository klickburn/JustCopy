# Identify the most common bucket within each customer segment
most_common_bucket_per_segment = events_df.groupby(['segment', 'bucket']).size().reset_index(name='frequency')
most_common_bucket_per_segment = most_common_bucket_per_segment.sort_values(['segment', 'frequency'], ascending=[True, False])
most_common_bucket_per_segment = most_common_bucket_per_segment.drop_duplicates('segment')

# Display the data
most_common_bucket_per_segment
