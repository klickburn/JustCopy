# Calculating the number of accounts enrolled in each type of payment program per bucket

# Grouping by bucket and program type, then counting the number of accounts
enrollments_per_bucket_program = merged_df.groupby(['bucket', 'program_type']).size().unstack(fill_value=0)

enrollments_per_bucket_program
