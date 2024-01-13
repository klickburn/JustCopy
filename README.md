def count_segment_changes(current_df, next_df):
    """
    Count the number of accounts for each segment that changed their segment in the next month.

    Args:
    current_df (pd.DataFrame): The dataframe of the current month.
    next_df (pd.DataFrame): The dataframe of the next month.

    Returns:
    dict: A dictionary with the count of segment changes for each segment.
    """
    # Merge the two dataframes on 'acct_ref_nb' to find common accounts
    merged_df = current_df[['acct_ref_nb', 'segment']].merge(next_df[['acct_ref_nb', 'segment']], on='acct_ref_nb', suffixes=('_current', '_next'))

    # Initialize a dictionary to count changes for each segment
    segment_changes = {segment: 0 for segment in current_df['segment'].unique()}

    # Count changes for each segment
    for segment in segment_changes:
        segment_changes[segment] = merged_df[(merged_df['segment_current'] == segment) & (merged_df['segment_next'] != segment)].shape[0]

    return segment_changes

# Example monthly dataframes: jan_df, feb_df, ..., dec_df

# Dictionary to store the segment changes for each month
monthly_segment_changes = {}

# List of dataframes for each month
monthly_dfs = [jan_df, feb_df, mar_df, apr_df, may_df, jun_df, jul_df, aug_df, sep_df, oct_df, nov_df, dec_df]

# Loop through the dataframes and calculate the segment changes
for i in range(len(monthly_dfs) - 1):
    month_name = monthly_dfs[i].columns.name  # Assuming the dataframe has a name attribute set to the month name
    next_month_name = monthly_dfs[i + 1].columns.name
    monthly_segment_changes[f"{month_name}_to_{next_month_name}"] = count_segment_changes(monthly_dfs[i], monthly_dfs[i + 1])

monthly_segment_changes
