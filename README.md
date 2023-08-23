# Identify accounts that made a payment
accounts_with_payment = events_df_filtered[events_df_filtered['channel'] == 'PYMT']['acct_ref_nb'].unique()

# For these accounts, list all unique channels they were contacted through before making a payment
channels_before_payment = events_df_filtered[events_df_filtered['acct_ref_nb'].isin(accounts_with_payment)].groupby('acct_ref_nb')['channel'].unique()

# Exclude PYMT from the list if there are multiple PYMT events for an account
channels_before_payment = channels_before_payment.apply(lambda x: [channel for channel in x if channel != 'PYMT' or list(x).count('PYMT') == 1])

# Assign equal credit to each channel in the list
channel_credits = channels_before_payment.explode().value_counts()

channel_credits
