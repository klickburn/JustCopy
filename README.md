def calculate_specific_roll_rates(df_current, df_next):
    # Identify accounts that are present in both months
    common_accounts = df_current[['acct_ref_nb', 'bucket']].merge(df_next[['acct_ref_nb', 'bucket']], on='acct_ref_nb', suffixes=('_current', '_next'))

    # Calculate specific roll rates (accounts that moved to the next higher bucket)
    specific_rolled = common_accounts[common_accounts['bucket_next'] == common_accounts['bucket_current'] + 1]
    specific_roll_counts = specific_rolled['bucket_current'].value_counts()

    # Calculate the total number of accounts in each bucket for the current month
    bucket_counts_current = df_current['bucket'].value_counts()

    # Calculate roll rates as a percentage
    specific_roll_rates = (specific_roll_counts / bucket_counts_current).fillna(0) * 100

    return specific_roll_rates.to_dict()

# Example calculation for January to February
jan_to_feb_specific_roll_rates = calculate_specific_roll_rates(jan_df, feb_df)

jan_to_feb_specific_roll_rates


