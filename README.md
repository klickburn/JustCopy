
# Find duplicate 'acct_ref_nb' in 'jan_df'
duplicate_acct_jan = jan_df['acct_ref_nb'].duplicated().sum()

# Monthly dataframes list
monthly_dfs = [jan_df, feb_df, mar_df, apr_df, may_df, jun_df, jul_df, aug_df, sep_df, oct_df, nov_df, dec_df]
monthly_totals = []

# Calculate total accounts for each month in millions
for df in monthly_dfs:
    total_accounts = len(df) / 1_000_000  # Convert to millions
    monthly_totals.append(total_accounts)

duplicate_acct_jan, monthly_totals

