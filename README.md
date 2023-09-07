# Calculate the total number of payments ("PYMT") within each account balance range
total_payments_per_balance_range = events_df[events_df['channel'] == 'PYMT'].groupby('balance_range').size().reset_index(name='total_payments')

# Display the data
total_payments_per_balance_range
