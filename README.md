def calculate_detailed_bucket_transitions(current_df, next_df):
    # Merge the two dataframes on 'acct_ref_nb' to find common accounts
    merged_df = current_df[['acct_ref_nb', 'bucket']].merge(next_df[['acct_ref_nb', 'bucket']], on='acct_ref_nb', suffixes=('_current', '_next'), how='left')

    # Find all unique buckets in current and next dataframes
    unique_buckets = set(current_df['bucket'].unique()).union(set(next_df['bucket'].unique()))

    # Initialize a dictionary to count transitions for each bucket pair
    bucket_transitions = {(bucket_current, bucket_next): 0 for bucket_current in unique_buckets for bucket_next in unique_buckets}

    # Count transitions for each bucket pair
    for bucket_current, bucket_next in bucket_transitions:
        bucket_transitions[(bucket_current, bucket_next)] = merged_df[(merged_df['bucket_current'] == bucket_current) & (merged_df['bucket_next'] == bucket_next)].shape[0]

    # Count stays for each bucket
    stayed_counts = {bucket: 0 for bucket in unique_buckets}
    for bucket in stayed_counts:
        stayed_counts[bucket] = merged_df[(merged_df['bucket_current'] == bucket) & (merged_df['bucket_next'] == bucket)].shape[0]

    return {'Transitions': bucket_transitions, 'Stayed': stayed_counts}

# Dictionary to store detailed bucket transitions and stays for each month
detailed_bucket_transitions_and_stays = {}

# Loop through the dataframes and calculate the detailed bucket transitions and stays
for i in range(len(monthly_dfs) - 1):
    current_month_name = monthly_dfs[i].columns.name
    next_month_name = monthly_dfs[i + 1].columns.name
    detailed_bucket_transitions_and_stays[f"{current_month_name}_to_{next_month_name}"] = calculate_detailed_bucket_transitions(monthly_dfs[i], monthly_dfs[i + 1])

detailed_bucket_transitions_and_stays

