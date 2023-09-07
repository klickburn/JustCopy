# Plot the distribution of channels within each account balance range
plt.figure(figsize=(16, 10))
sns.countplot(x='balance_range', hue='channel', data=events_df, palette='viridis')
plt.title('Frequency of Each Channel within Each Account Balance Range')
plt.xlabel('Account Balance Range ($)')
plt.ylabel('Frequency')
plt.legend(title='Channel', loc='upper right')
plt.show()
