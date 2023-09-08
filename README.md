events_df['next_date'] = events_df.groupby('acct_ref_nb')['date'].shift(-1)
events_df['next_channel'] = events_df.groupby('acct_ref_nb')['channel'].shift(-1)

# Filtering for the desired sequence and date difference
outbound_then_pymt = events_df[
    (events_df['channel'] == 'OUTBOUND') & 
    (events_df['next_channel'] == 'PYMT') & 
    ((events_df['next_date'] - events_df['date']).dt.days <= 30)
]

# Counting unique accounts
unique_outbound_then_pymt_30days = outbound_then_pymt['acct_ref_nb'].nunique()

unique_outbound_then_pymt_30days
