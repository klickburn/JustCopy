def calculate_bucket_percentages_and_roll_rates(df_current, df_next):
    # Calculate percentage of accounts in each bucket for the current month
    bucket_counts_current = df_current['bucket'].value_counts()
    total_accounts_current = len(df_current)
    bucket_percentages_current = (bucket_counts_current / total_accounts_current) * 100
    return bucket_percentages_current.to_dict()
