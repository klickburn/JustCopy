# Calculate the total account balance in each segment
total_balance_per_segment = jan_df.groupby('segment')['acct_balance'].sum()

# Calculate the total account balance across all segments
total_balance = total_balance_per_segment.sum()

# Calculate the percentage of total account balance in each segment
balance_percentage_per_segment = (total_balance_per_segment / total_balance) * 100
balance_percentage_per_segment_dict = balance_percentage_per_segment.to_dict()
balance_percentage_per_segment_dict
