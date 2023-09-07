# Insight 7: Examine the distribution of customer segments within each bucket for different account balance ranges
plt.figure(figsize=(16, 10))
sns.countplot(x='bucket', hue='segment', data=events_df, palette='viridis')
plt.title('Distribution of Customer Segments within Each Bucket')
plt.xlabel('Bucket')
plt.ylabel('Frequency')
plt.legend(title='Segment', loc='upper right')
plt.show()
