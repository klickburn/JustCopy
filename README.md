# Insight 2: Calculate the average account balance for each unique cluster
average_balance_per_cluster = events_df.groupby('cluster_id')['acct_balance'].mean().reset_index(name='average_balance')

# Display the top 10 clusters based on average account balance
average_balance_per_cluster.sort_values('average_balance', ascending=False).head(10)
