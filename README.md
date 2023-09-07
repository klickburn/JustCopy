# Create a new column that combines bucket, balance_range, and segment into a single identifier for each unique cluster
events_df['cluster_id'] = events_df['bucket'].astype(str) + "_" + events_df['balance_range'].astype(str) + "_" + events_df['segment'].astype(str)

# Insight 1: Most Effective Channel for Each Combination of Bucket, Account Balance Range, and Segment
# For the purpose of this analysis, we'll consider the "PYMT" channel as the measure of effectiveness.

# Calculate the total number of "PYMT" for each cluster
total_payments_per_cluster = events_df[events_df['channel'] == 'PYMT'].groupby('cluster_id').size().reset_index(name='total_payments')

# Calculate the total number of contacts for each cluster
total_contacts_per_cluster = events_df.groupby('cluster_id').size().reset_index(name='total_contacts')

# Merge the total_payments_per_cluster and total_contacts_per_cluster DataFrames
ratio_cluster_df = pd.merge(total_payments_per_cluster, total_contacts_per_cluster, on='cluster_id', how='outer').fillna(0)

# Calculate the ratio of payments to contacts for each cluster
ratio_cluster_df['payment_to_contact_ratio'] = ratio_cluster_df['total_payments'] / ratio_cluster_df['total_contacts']

# Sort the DataFrame based on the ratio to find the most effective channel for each cluster
most_effective_cluster = ratio_cluster_df.sort_values('payment_to_contact_ratio', ascending=False).drop_duplicates('cluster_id')

# Display the top 10 most effective clusters
most_effective_cluster.head(10)
