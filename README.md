# Step 1: For each acct_ref_nb, shift the channel column to get the previous channel before each entry
# This step was already done in the previous code, so we can reuse the 'previous_channel' column

# Step 2: Filter rows where the current channel is 'PROMISE' and then count occurrences of each previous channel
promise_after_channel = events_df[events_df['channel'] == 'PROMISE']['previous_channel'].value_counts()

# Step 3: Count total volume of PROMISE events
total_promise_volume = events_df[events_df['channel'] == 'PROMISE'].shape[0]

promise_after_channel, total_promise_volume
