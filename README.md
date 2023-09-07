# Plot the distribution of customer segments within each bucket
plt.figure(figsize=(16, 10))
sns.countplot(x='bucket', hue='segment', data=events_df, palette='viridis')
plt.title('Frequency of Each Segment within Each Bucket')
plt.xlabel('Bucket')
plt.ylabel('Frequency')
plt.legend(title='Segment', loc='upper right')
plt.show()
