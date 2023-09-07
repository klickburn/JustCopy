# Plot the distribution of customer segments by channel
plt.figure(figsize=(16, 10))
sns.countplot(x='channel', hue='segment', data=events_df, palette='viridis')
plt.title('Channel Usage by Customer Segment')
plt.xlabel('Channel')
plt.ylabel('Frequency')
plt.legend(title='Segment', loc='upper right')
plt.show()
