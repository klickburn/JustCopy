# Code to calculate the average number of calls an account receives in each bucket
average_calls_per_bucket = calls_details_df.groupby('bucket').apply(lambda x: x['acct_ref_nb'].value_counts().mean())

average_calls_per_bucket
