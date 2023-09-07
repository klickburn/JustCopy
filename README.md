unique_accts_per_segment = events_df.groupby('segment')['acct_ref_nb'].nunique().reset_index()
unique_accts_per_segment.columns = ['segment', 'unique_acct_ref_nb_count']
print(unique_accts_per_segment)
