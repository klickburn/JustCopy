
# Calculating the bucket for each account at the time of enrolling in a payment program
# Each bucket represents a 30-day period of delinquency
merged_df = pd.merge(chargeoffs_df, pmt_program_df, on='ACCT_REF_NB')
merged_df['days_in_delinquency'] = (merged_df['pgm_dt'] - merged_df['Dlq_Date']).dt.days
merged_df['bucket'] = (merged_df['days_in_delinquency'] // 30) + 1  # Bucket 1: 0-30 days, Bucket 2: 31-60 days, etc.

# Determining in which bucket each account enrolled in a payment program
enrollment_buckets = merged_df.groupby('ACCT_REF_NB')['bucket'].first()

enrollment_buckets.head()
