# Function to compute time difference between consecutive actions for each customer
def time_between_actions(group):
    group['time_diff'] = group['date'].diff().dt.days
    return group

# Apply the function to each customer's data
modified_events_df_with_diff = modified_events_df.groupby('acct_ref_nb').apply(time_between_actions)

# Compute average time difference for each customer
avg_time_diff_per_customer = modified_events_df_with_diff.groupby('acct_ref_nb')['time_diff'].mean()

# Overall average time difference across all customers
overall_avg_time_diff = avg_time_diff_per_customer.mean()

overall_avg_time_diff
