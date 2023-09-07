# Insight 3: Identify the most frequently used channel within each unique cluster
most_frequent_channel_per_cluster = events_df.groupby(['cluster_id', 'channel']).size().reset_index(name='frequency')
most_frequent_channel_per_cluster = most_frequent_channel_per_cluster.sort_values(['cluster_id', 'frequency'], ascending=[True, False])
most_frequent_channel_per_cluster = most_frequent_channel_per_cluster.drop_duplicates('cluster_id')

# Display the top 10 clusters based on most frequent channel
most_frequent_channel_per_cluster.head(10)
