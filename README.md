# Adding columns for the next and next-next channels
events_df['next_channel'] = events_df.groupby('acct_ref_nb')['channel'].shift(-1)
events_df['next_next_channel'] = events_df.groupby('acct_ref_nb')['channel'].shift(-2)

# Filtering based on conditions
outbound_login_pymt = events_df[
    (events_df['channel'] == 'OUTBOUND') & 
    (events_df['next_channel'] == 'LOGIN') & 
    (events_df['next_next_channel'] == 'PYMT')
]
unique_outbound_login_pymt = outbound_login_pymt['acct_ref_nb'].nunique()

outbound_promise_pymt = events_df[
    (events_df['channel'] == 'OUTBOUND') & 
    (events_df['next_channel'] == 'PROMISE') & 
    (events_df['next_next_channel'] == 'PYMT')
]
unique_outbound_promise_pymt = outbound_promise_pymt['acct_ref_nb'].nunique()

unique_outbound_login_pymt, unique_outbound_promise_pymt
