# Merging payment_df with chargeoffs_df to get the delinquency bucket information
merged_payments_chargeoffs = pd.merge(payment_df, chargeoffs_df[['ACCT_REF_NB', 'Dlq_Date']], on='ACCT_REF_NB')

# Calculating the bucket for each payment based on the delinquency date
merged_payments_chargeoffs['days_in_delinquency'] = (merged_payments_chargeoffs['payment_date'] - merged_payments_chargeoffs['Dlq_Date']).dt.days
merged_payments_chargeoffs['bucket'] = (merged_payments_chargeoffs['days_in_delinquency'] // 30) + 1  # Bucket 1: 0-30 days, etc.

# Counting the unique accounts making payments in each bucket
unique_accounts_per_payment_bucket = merged_payments_chargeoffs.groupby('bucket')['ACCT_REF_NB'].nunique()

unique_accounts_per_payment_bucket
