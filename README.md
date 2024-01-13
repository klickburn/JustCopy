# Function to count detailed segment transitions between months
def count_detailed_segment_transitions(current_df, next_df):
    # Merge the two dataframes on 'acct_ref_nb' to find common accounts
    merged_df = current_df[['acct_ref_nb', 'segment']].merge(next_df[['acct_ref_nb', 'segment']], on='acct_ref_nb', suffixes=('_current', '_next'))

    # Find all unique segments in current and next dataframes
    unique_segments = set(current_df['segment'].unique()).union(set(next_df['segment'].unique()))

    # Initialize a dictionary to count transitions for each segment pair
    segment_transitions = {(seg_current, seg_next): 0 for seg_current in unique_segments for seg_next in unique_segments if seg_current != seg_next}

    # Count transitions for each segment pair
    for seg_current, seg_next in segment_transitions:
        segment_transitions[(seg_current, seg_next)] = merged_df[(merged_df['segment_current'] == seg_current) & (merged_df['segment_next'] == seg_next)].shape[0]

    return segment_transitions

# Dictionary to store detailed segment transitions for each month
detailed_monthly_segment_transitions = {}

# Loop through the dataframes and calculate the detailed segment transitions
for i in range(len(monthly_dfs) - 1):
    current_month_name = monthly_dfs[i].columns.name
    next_month_name = monthly_dfs[i + 1].columns.name
    detailed_monthly_segment_transitions[f"{current_month_name}_to_{next_month_name}"] = count_detailed_segment_transitions(monthly_dfs[i], monthly_dfs[i + 1])

detailed_monthly_segment_transitions

