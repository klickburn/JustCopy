# Plot the distribution of account balances within each bucket
plt.figure(figsize=(16, 10))
sns.boxplot(x='bucket', y='acct_balance', data=events_df)
plt.title('Distribution of Account Balances per Bucket')
plt.xlabel('Bucket')
plt.ylabel('Account Balance ($)')
plt.show()
