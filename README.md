# Calculate the time taken from the first contact to payment for each account
time_to_payment = events_df_filtered[events_df_filtered['acct_ref_nb'].isin(accounts_with_payment)].groupby('acct_ref_nb').agg({
    'date': ['min', 'max']
})

time_to_payment.columns = ['first_contact_date', 'payment_date']
time_to_payment['days_to_payment'] = (time_to_payment['payment_date'] - time_to_payment['first_contact_date']).dt.days

# Get the first contact channel for each account
first_contact_channel = events_df_filtered.groupby('acct_ref_nb').first()['channel']

# Merge with the time_to_payment dataframe
time_to_payment = time_to_payment.merge(first_contact_channel, left_index=True, right_index=True)

# Calculate the weighted average time to payment for each channel based on the time-decay credit
weighted_time_to_payment = credits_df.merge(time_to_payment, left_on='acct_ref_nb', right_index=True)
weighted_avg_time = (weighted_time_to_payment.groupby('variable').apply(lambda x: (x['credit'] * x['days_to_payment']).sum()) / weighted_time_to_payment.groupby('variable')['credit'].sum())

weighted_avg_time
