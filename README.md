# Calculate total accounts contacted per channel
total_accounts_per_channel = events_df_filtered.groupby('first_contact_channel')['acct_ref_nb'].nunique()

# Calculate success rate: Number of accounts that made a payment / Total accounts contacted per channel
success_rate = payment_accounts_per_channel / total_accounts_per_channel

# Plot the success rate
plt.figure(figsize=(10, 6))
success_rate.sort_values(ascending=False).plot(kind='bar', color='goldenrod')
plt.title('Success Rate of Each Channel Leading to Payment')
plt.xlabel('Channel')
plt.ylabel('Success Rate')
plt.grid(axis='y')
plt.show()
