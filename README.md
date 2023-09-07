# Calculate the average account balance within each bucket
average_balance_per_bucket = events_df.groupby('bucket')['acct_balance'].mean().reset_index(name='average_balance')

# Display the data
average_balance_per_bucket
