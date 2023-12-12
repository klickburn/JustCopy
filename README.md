# Calculating the bucket for each charge-off based on the delinquency date (Dlq_Date) and the charge-off complete date (CHRGF_CPLT_DT)
chargeoffs_df['days_to_chargeoff'] = (chargeoffs_df['CHRGF_CPLT_DT'] - chargeoffs_df['Dlq_Date']).dt.days
chargeoffs_df['bucket'] = (chargeoffs_df['days_to_chargeoff'] // 30) + 1  # Bucket 1: 0-30 days, etc.

chargeoffs_df.head()


# Calculating the total number of charge-off accounts in each bucket and their percentage based on total number of accounts

# Counting the number of charge-off accounts in each bucket
charged_off_counts_per_bucket = chargeoffs_df.groupby('bucket')['ACCT_REF_NB'].nunique()

# Calculating the total number of accounts in chargeoffs_df
total_number_of_accounts = chargeoffs_df['ACCT_REF_NB'].nunique()

# Calculating the percentage of charged off accounts in each bucket
percentage_per_bucket = (charged_off_counts_per_bucket / total_number_of_accounts) * 100

# Combining counts and percentages
charged_off_summary = pd.DataFrame({
    'Total Charged Off': charged_off_counts_per_bucket,
    'Percentage': percentage_per_bucket
})

charged_off_summary
