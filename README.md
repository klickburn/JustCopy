# Convert 'bucket' to numeric in feb_df
feb_df['bucket'] = pd.to_numeric(feb_df['bucket'], errors='coerce').fillna(1).astype(int)

# Merge jan_df and feb_df on 'acct_ref_nb'
merged_df = jan_df[jan_df['bucket'] == 1].merge(feb_df, on='acct_ref_nb', suffixes=('_jan', '_feb'))

# Calculate the number of accounts that moved from bucket 1 in January to bucket 2 in February, in each segment
moved_to_bucket_2 = merged_df[(merged_df['bucket_feb'] == 2)].groupby('segment_jan').size()

# Calculate the total number of accounts in each segment in January that were in bucket 1
total_in_bucket_1 = jan_df[jan_df['bucket'] == 1].groupby('segment').size()

# Calculate the percentage of accounts in each segment that moved from bucket 1 to bucket 2
percentage_moved_to_bucket_2 = (moved_to_bucket_2 / total_in_bucket_1) * 100
percentage_moved_to_bucket_2_dict = percentage_moved_to_bucket_2.fillna(0).to_dict()
percentage_moved_to_bucket_2_dict
