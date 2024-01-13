# Example dataframes (to be replaced with actual dataframes)
# jan_df = pd.DataFrame(...)
# feb_df = pd.DataFrame(...)
# ... and so on for other months

# Dictionary to hold the counts of segment changes for each month
segment_change_counts = {}

# Function to count segment changes compared to jan_df
def count_segment_changes(jan_df, other_df, month):
    # Merging jan_df with the other dataframe on 'acct_ref_nb'
    merged_df = jan_df[['acct_ref_nb', 'segment']].merge(other_df[['acct_ref_nb', 'segment']], on='acct_ref_nb', suffixes=('_jan', '_other'))
    
    # Counting where segments have changed
    segment_changes = (merged_df['segment_jan'] != merged_df['segment_other']).sum()
    return segment_changes

# Loop through each month's dataframe
for month, df in zip(['Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'], 
                     [feb_df, mar_df, apr_df, may_df, jun_df, jul_df, aug_df, sep_df, oct_df, nov_df, dec_df]):
    segment_change_counts[month] = count_segment_changes(jan_df, df, month)

segment_change_counts
