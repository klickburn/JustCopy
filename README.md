# Adjusting the approach for Insight 2

# First, merge communication data (comms_df) with charge-off data (chargeoffs_df)
merged_comm_chargeoff = pd.merge(comms_df, chargeoffs_df, left_on='acct_ref_nb', right_on='ACCT_REF_NB')

# Filtering to include only communications before the charge-off date
merged_comm_chargeoff_filtered = merged_comm_chargeoff[merged_comm_chargeoff['comm_dt'] <= merged_comm_chargeoff['CHRGF_CPLT_DT']]

# Counting the types of communication for each account
communication_counts = merged_comm_chargeoff_filtered.groupby(['acct_ref_nb', 'COMMUNICATION_TYPE']).size().unstack(fill_value=0)

# Now, merging with calls data (calls_df)
merged_with_calls = pd.merge(calls_df, communication_counts, left_on='ACCT_REF_NB', right_on='acct_ref_nb', how='left')

# Filling NaN values with 0, as no communication means zero counts
merged_with_calls_filled = merged_with_calls.fillna(0)

# Aggregating the communication data
communication_aggregated = merged_with_calls_filled.groupby('ACCT_REF_NB').sum()

communication_aggregated.head()
