# Correct approach to calculate the average number of each communication type sent out in each bucket

# Grouping by bucket and communication type, then counting the occurrences
grouped_comms = merged_comms_chargeoffs.groupby(['bucket', 'COMMUNICATION_TYPE']).size()

# Calculating the average for each communication type in each bucket
# First, unstack to separate communication types into different columns
grouped_comms_unstacked = grouped_comms.unstack('COMMUNICATION_TYPE', fill_value=0)
# First, we will calculate the total unique accounts that received each communication type in each bucket
average_comms_per_type_per_bucket = grouped_comms_unstacked.mean()

# Counting the unique accounts for each communication type in each bucket
unique_accounts_per_comms_type = merged_comms_chargeoffs.groupby(['bucket', 'COMMUNICATION_TYPE'])['acct_ref_nb'].nunique().unstack(fill_value=0)

# Now, dividing the average_comms_per_bucket_and_type_df by unique_accounts_per_comms_type to get the average per account
average_comms_per_account = average_comms_per_type_per_bucket.div(unique_accounts_per_comms_type)

average_comms_per_account
