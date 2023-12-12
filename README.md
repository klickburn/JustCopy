merged_payments = pd.merge(payment_df, chargeoffs_df[['ACCT_REF_NB', 'Dlq_Date']], on='ACCT_REF_NB')

# Calculating the bucket for each payment based on the delinquency date
merged_payments['days_in_delinquency'] = (merged_payments['payment_date'] - merged_payments['Dlq_Date']).dt.days
merged_payments['bucket'] = (merged_payments['days_in_delinquency'] // 30) + 1  # Bucket 1: 0-30 days, etc.

# Updating payment_df with the new 'bucket' information
payment_df['bucket'] = merged_payments['bucket']

payment_df.head()

# Calculating the total payments received in each bucket

total_payments_per_bucket = payment_df.groupby('bucket')['payment_amt'].sum()
total_payments_per_bucket
