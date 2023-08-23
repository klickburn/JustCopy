# Calculate the time taken from the first contact to payment
events_df_filtered['date_first_contact'] = events_df_filtered.groupby('acct_ref_nb')['date'].transform('first')
events_df_filtered['days_to_payment'] = np.where(events_df_filtered['channel'] == 'PYMT', 
                                                (events_df_filtered['date'] - events_df_filtered['date_first_contact']).dt.days, 
                                                np.nan)

# Calculate average days to payment per channel
avg_days_to_payment = events_df_filtered.groupby('first_contact_channel')['days_to_payment'].mean().dropna()

# Plot the distribution
plt.figure(figsize=(10, 6))
avg_days_to_payment.plot(kind='bar', color='lightgreen')
plt.title('Average Days from First Contact to Payment per Channel')
plt.xlabel('Channel')
plt.ylabel('Average Days')
plt.grid(axis='y')
plt.show()
