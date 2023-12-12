# Calculating the bucket for each charge-off based on the delinquency date (Dlq_Date) and the charge-off complete date (CHRGF_CPLT_DT)
chargeoffs_df['days_to_chargeoff'] = (chargeoffs_df['CHRGF_CPLT_DT'] - chargeoffs_df['Dlq_Date']).dt.days
chargeoffs_df['bucket'] = (chargeoffs_df['days_to_chargeoff'] // 30) + 1  # Bucket 1: 0-30 days, etc.

chargeoffs_df.head()
