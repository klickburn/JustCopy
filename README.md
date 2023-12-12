# Correcting the calculation approach to get the average number of each communication type sent out in each bucket

# Grouping by bucket and communication type, then counting the occurrences
grouped_comms_per_bucket = merged_comms_chargeoffs.groupby(['bucket', 'COMMUNICATION_TYPE']).size()

# Calculating the average for each communication type in each bucket
average_comms_per_bucket_and_type_df = grouped_comms_per_bucket.unstack(fill_value=0)

average_comms_per_bucket_and_type_df
