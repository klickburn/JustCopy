
# Function to calculate bucket changes, exits, and percentages
def calculate_bucket_changes_and_exits_percentage(current_df, next_df):
    # Merge the two dataframes on 'acct_ref_nb' to find common accounts
    merged_df = current_df[['acct_ref_nb', 'bucket']].merge(next_df[['acct_ref_nb', 'bucket']], on='acct_ref_nb', suffixes=('_current', '_next'), how='left')

    # Total number of accounts in the current month
    total_accounts_current_month = len(current_df)

    # Calculate rolled ahead, stayed, and got out
    rolled_ahead = merged_df[merged_df['bucket_current'] != merged_df['bucket_next']].shape[0]
    stayed = merged_df[merged_df['bucket_current'] == merged_df['bucket_next']].shape[0]
    got_out = merged_df[merged_df['bucket_next'].isnull()].shape[0]

    # Calculate percentages
    rolled_ahead_percentage = (rolled_ahead / total_accounts_current_month) * 100
    stayed_percentage = (stayed / total_accounts_current_month) * 100
    got_out_percentage = (got_out / total_accounts_current_month) * 100

    return {'Rolled Ahead': rolled_ahead_percentage, 'Stayed': stayed_percentage, 'Got Out': got_out_percentage}

# List of dataframes for each month
monthly_dfs = [jan_df, feb_df, mar_df, apr_df, may_df, jun_df, jul_df, aug_df, sep_df, oct_df, nov_df, dec_df]

# Dictionary to store the bucket changes and exits percentages for each month
bucket_changes_and_exits_percentages = {}

# Loop through the dataframes and calculate the bucket changes and exits percentages
for i in range(len(monthly_dfs) - 1):
    current_month_name = monthly_dfs[i].columns.name
    next_month_name = monthly_dfs[i + 1].columns.name
    bucket_changes_and_exits_percentages[f"{current_month_name}_to_{next_month_name}"] = calculate_bucket_changes_and_exits_percentage(monthly_dfs[i], monthly_dfs[i + 1])

bucket_changes_and_exits_percentages
