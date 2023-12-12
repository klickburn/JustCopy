# Adding a 'bucket' column to comms_df based on the delinquency date from chargeoffs_df

# First, we need to merge comms_df with chargeoffs_df to get the delinquency date for each account
merged_comms_chargeoffs = pd.merge(comms_df, chargeoffs_df[['ACCT_REF_NB', 'Dlq_Date']], left_on='acct_ref_nb', right_on='ACCT_REF_NB')

# Calculating the bucket for each communication based on the delinquency date
merged_comms_chargeoffs['days_in_delinquency'] = (merged_comms_chargeoffs['comm_dt'] - merged_comms_chargeoffs['Dlq_Date']).dt.days
merged_comms_chargeoffs['bucket'] = (merged_comms_chargeoffs['days_in_delinquency'] // 30) + 1  # Bucket 1: 0-30 days, etc.
