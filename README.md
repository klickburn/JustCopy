# Plot the distribution of delinquency buckets by channel
plt.figure(figsize=(14, 8))
sns.countplot(x='channel', hue='bucket', data=events_df, palette='viridis')
plt.title('Distribution of Delinquency Buckets by Channel')
plt.xlabel('Channel')
plt.ylabel('Frequency')
plt.legend(title='Bucket', loc='upper right')
plt.show()
