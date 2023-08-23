# Sort the dataframe by acct_ref_nb and date
events_df_sorted = events_df.sort_values(by=['acct_ref_nb', 'date'])

# Create a new column for the first contact channel
events_df_sorted['first_contact_channel'] = events_df_sorted.groupby('acct_ref_nb')['channel'].transform('first')

# Filter out rows where the first contact channel is PYMT
events_df_filtered = events_df_sorted[events_df_sorted['first_contact_channel'] != 'PYMT']

events_df_filtered.head()


# Accounts that made a payment
payment_accounts = events_df_filtered[events_df_filtered['channel'] == 'PYMT']['acct_ref_nb'].unique()

# Filter the dataframe for these accounts and their first contact channels
first_contact_channels_payment = events_df_filtered[events_df_filtered['acct_ref_nb'].isin(payment_accounts)]['first_contact_channel'].value_counts()

# Plot the distribution
plt.figure(figsize=(10, 6))
first_contact_channels_payment.plot(kind='bar', color='skyblue')
plt.title('Distribution of First Contact Channel for Accounts that Made a Payment')
plt.xlabel('Channel')
plt.ylabel('Number of Accounts')
plt.grid(axis='y')
plt.show()
