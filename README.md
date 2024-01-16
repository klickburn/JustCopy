def calculate_acct_balance_move_ahead_percentage(current_df, next_df):
    # Merge the two dataframes on 'acct_ref_nb' to find common accounts
    merged_df = current_df[['acct_ref_nb', 'bucket', 'acct_balance']].merge(
        next_df[['acct_ref_nb', 'bucket']], on='acct_ref_nb', suffixes=('_current', '_next'))

    # Filter for accounts that moved to a different bucket
    moved_accounts = merged_df[merged_df['bucket_current'] != merged_df['bucket_next']]

    # Sum of account balances that moved to a different bucket
    moved_balances = moved_accounts.groupby('bucket_current')['acct_balance'].sum()

    # Total account balances in each bucket for the current month
    total_balances = current_df.groupby('bucket')['acct_balance'].sum()

    # Calculate the move ahead percentages
    move_ahead_percentage = (moved_balances / total_balances) * 100

    return move_ahead_percentage.fillna(0).to_dict()

# Example usage
# Assuming monthly_dfs is a list of dataframes for each month
acct_balance_move_ahead_percentages = {}
for i in range(len(monthly_dfs) - 1):
    current_month_name = monthly_dfs[i].columns.name
    next_month_name = monthly_dfs[i + 1].columns.name
    acct_balance_move_ahead_percentages[f"{current_month_name}_to_{next_month_name}"] = calculate_acct_balance_move_ahead_percentage(monthly_dfs[i], monthly_dfs[i +