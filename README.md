# Calculate the total number of payments ("PYMT") within each customer segment
total_payments_per_segment = events_df[events_df['channel'] == 'PYMT'].groupby('segment').size().reset_index(name='total_payments')

# Display the data
total_payments_per_segment
