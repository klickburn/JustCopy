# Plot the distribution of account balances resulting in payment ("PYMT")
plt.figure(figsize=(14, 8))
sns.histplot(events_df[events_df['channel'] == 'PYMT']['acct_balance'], bins=20, kde=True)
plt.title('Distribution of Account Balances Resulting in Payment')
plt.xlabel('Account Balance ($)')
plt.ylabel('Frequency')
plt.show()
