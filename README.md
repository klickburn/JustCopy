import matplotlib.pyplot as plt
import seaborn as sns

# Set the style for the plots
sns.set(style="whitegrid")

# Plot the distribution of account balances by channel
plt.figure(figsize=(14, 8))
sns.boxplot(x='channel', y='acct_balance', data=events_df)
plt.title('Distribution of Account Balances by Channel')
plt.xlabel('Channel')
plt.ylabel('Account Balance ($)')
plt.show()

