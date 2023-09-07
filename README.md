# Plot the distribution of delinquency buckets within each account balance range
plt.figure(figsize=(16, 10))
sns.countplot(x='balance_range', hue='bucket', data=events_df, palette='viridis')
plt.title('Distribution of Delinquency Buckets for Different Account Balance Ranges')
plt.xlabel('Account Balance Range ($)')
plt.ylabel('Frequency')
plt.legend(title='Bucket', loc='upper right')
plt.show()
