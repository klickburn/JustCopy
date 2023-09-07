# Calculate the total number of payments ("PYMT") within each bucket
total_payments_per_bucket = events_df[events_df['channel'] == 'PYMT'].groupby('bucket').size().reset_index(name='total_payments')

# Display the data
total_payments_per_bucket
