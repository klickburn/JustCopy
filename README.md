# First, identify accounts that were in bucket 1 in January (jan_df) and are not present in February (feb_df)
jan_bucket_1 = jan_df[jan_df['bucket'] == 1]
missing_in_feb = jan_bucket_1[~jan_bucket_1['acct_ref_nb'].isin(feb_df['acct_ref_nb'])]

# Calculate the number of such accounts in each segment
missing_counts_per_segment = missing_in_feb['segment'].value_counts()

# Calculate the total number of accounts in each segment in January that were in bucket 1
total_in_bucket_1_per_segment = jan_bucket_1['segment'].value_counts()

# Calculate the percentage of accounts in each segment that were in bucket 1 in January and are not present in February
percentage_missing_in_feb = (missing_counts_per_segment / total_in_bucket_1_per_segment) * 100
percentage_missing_in_feb_dict = percentage_missing_in_feb.fillna(0).to_dict()
percentage_missing_in_feb_dict

