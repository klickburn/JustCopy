def get_refined_first_touchpoint(group):
    if 'pymt' in group['channel'].values:
        for _, row in group.iterrows():
            if row['channel'] in channels:
                return row['channel']
    return None

# Applying the refined function to each group of acct_ref_nb
refined_first_touchpoints = modified_events_df.groupby('acct_ref_nb').apply(get_refined_first_touchpoint)

# Counting the occurrences of each first touchpoint leading to a payment
refined_first_touchpoint_counts = refined_first_touchpoints.value_counts()
refined_first_touchpoint_counts
