# Grouping accounts according to Segment and finding total charged-off amount and total number of accounts for each segment

# Grouping by Segment
segment_grouped = chargeoffs_df.groupby('Segment').agg({'CHRGF_BAL_AM': 'sum', 'ACCT_REF_NB': 'count'})

# Renaming columns for clarity
segment_grouped = segment_grouped.rename(columns={'CHRGF_BAL_AM': 'Total Charged-Off Amount', 'ACCT_REF_NB': 'Total Accounts'})

segment_grouped
