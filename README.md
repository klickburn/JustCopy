# Adjust the channels_before_payment to remove PYMT from the list
channels_before_payment = channels_before_payment.apply(lambda x: [channel for channel in x if channel != 'PYMT'])

# Recalculate time-decay credits without PYMT
time_decay_credits_per_account = channels_before_payment.apply(assign_time_decay_credits)

# Convert the dictionary of credits to a dataframe for easier aggregation
credits_df = time_decay_credits_per_account.apply(pd.Series).reset_index().melt(id_vars='acct_ref_nb', value_name='credit').dropna()

# Aggregate to get total time-decay credits for each channel
total_time_decay_credits = credits_df.groupby('variable')['credit'].sum()

total_time_decay_credits
