# Step 1: For each acct_ref_nb, shift the channel column to get the previous channel before each entry
events_df['previous_channel'] = events_df.groupby('acct_ref_nb')['channel'].shift(1)

# Step 2: Filter rows where the current channel is 'PYMT' and then count occurrences of each previous channel
payments_after_channel = events_df[events_df['channel'] == 'PYMT']['previous_channel'].value_counts()

# Step 3: Count total volume of PYMT events
total_pymt_volume = events_df[events_df['channel'] == 'PYMT'].shape[0]

payments_after_channel, total_pymt_volume
