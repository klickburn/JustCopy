events_df = events_df.sort_values(by=["acct_ref_nb", "date"])

events_df.head(10)

# Adding a column for the previous channel
events_df['previous_channel'] = events_df.groupby('acct_ref_nb')['channel'].shift(1)

# Filtering based on conditions
login_after_outbound = events_df[(events_df['channel'] == 'LOGIN') & (events_df['previous_channel'] == 'OUTBOUND')]
unique_login_after_outbound = login_after_outbound['acct_ref_nb'].nunique()

# Note: PROMISE is not in our sample data, so we won't be able to determine its count
# However, I'll provide the code for completeness
promise_after_outbound = events_df[(events_df['channel'] == 'PROMISE') & (events_df['previous_channel'] == 'OUTBOUND')]
unique_promise_after_outbound = promise_after_outbound['acct_ref_nb'].nunique()

pymt_after_outbound = events_df[(events_df['channel'] == 'PYMT') & (events_df['previous_channel'] == 'OUTBOUND')]
unique_pymt_after_outbound = pymt_after_outbound['acct_ref_nb'].nunique()

unique_login_after_outbound, unique_promise_after_outbound, unique_pymt_after_outbound
