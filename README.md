# For Insight 3: Channel Switching, we'll investigate if switching channels during the customer journey leads to increased payments.

# Create a new column to hold the previous channel for each account
sorted_events_df_large['prev_channel'] = sorted_events_df_large.groupby('acct_ref_nb')['channel'].shift(1)

# Create a flag to identify when a channel switch occurs
sorted_events_df_large['channel_switched'] = sorted_events_df_large['channel'] != sorted_events_df_large['prev_channel']

# Filter the DataFrame to only include records where a channel switch occurred
channel_switch_df = sorted_events_df_large[sorted_events_df_large['channel_switched']]

# Filter the DataFrame further to only include records where a payment was made after a channel switch
channel_switch_to_payment_df = channel_switch_df[channel_switch_df['channel'] == 'PYMT']

# Count the number of times a payment was made after a channel was switched
channel_switch_to_payment_count = channel_switch_to_payment_df['prev_channel'].value_counts().reset_index()
channel_switch_to_payment_count.columns = ['prev_channel', 'payment_after_switch_count']

# Display the counts
channel_switch_to_payment_count
