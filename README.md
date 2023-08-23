# Number of accounts that made a payment per first_contact_channel
payment_accounts_per_channel = events_df_filtered[(events_df_filtered['channel'] == 'PYMT') & 
                                                  (events_df_filtered['acct_ref_nb'].isin(payment_accounts))].groupby('first_contact_channel')['acct_ref_nb'].nunique()

# Plot the distribution
plt.figure(figsize=(10, 6))
payment_accounts_per_channel.plot(kind='bar', color='lightcoral')
plt.title('Number of Accounts that Made a Payment per First Contact Channel')
plt.xlabel('Channel')
plt.ylabel('Number of Accounts')
plt.grid(axis='y')
plt.show()
