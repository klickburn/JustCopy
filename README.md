# Calculate the average account balance within each customer segment
average_balance_per_segment = events_df.groupby('segment')['acct_balance'].mean().reset_index(name='average_balance')

# Display the data
average_balance_per_segment
