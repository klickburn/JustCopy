# Insight 5: Examine the monthly trends of contacts for some of the most effective clusters
# Selecting a few top clusters based on the payment-to-contact ratio
top_clusters = most_effective_cluster.sort_values('payment_to_contact_ratio', ascending=False).head(5)['cluster_id'].tolist()

# Filter the events_df to only include these top clusters
top_clusters_df = events_df[events_df['cluster_id'].isin(top_clusters)]

# Plot the monthly trend of contacts for these top clusters
plt.figure(figsize=(16, 10))
sns.countplot(x='date_month_year', hue='cluster_id', data=top_clusters_df, palette='viridis')
plt.title('Monthly Trends of Contacts for Top Effective Clusters')
plt.xlabel('Month-Year')
plt.ylabel('Frequency')
plt.legend(title='Cluster ID', loc='upper left')
plt.xticks(rotation=45)
plt.show()
