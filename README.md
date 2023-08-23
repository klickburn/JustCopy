# Define a function to assign time-decay credits
def assign_time_decay_credits(channels_list):
    n = len(channels_list)
    # The sum of a geometric series: sum(0.5^i) for i from 0 to n is (1 - 0.5^(n+1)) / (1 - 0.5)
    total_credit = (1 - 0.5**(n+1)) / 0.5
    weights = [(0.5**i) / total_credit for i in range(n)]
    credits_dict = {channels_list[i]: weights[::-1][i] for i in range(n)}
    return credits_dict

# Assign time-decay credits to channels for each account
time_decay_credits_per_account = channels_before_payment.apply(assign_time_decay_credits)

# Explode the series to make aggregations easier
time_decay_credits_df = time_decay_credits_per_account.explode()

# Convert the dictionary of credits to a dataframe for easier aggregation
credits_df = time_decay_credits_per_account.apply(pd.Series).reset_index().melt(id_vars='acct_ref_nb', value_name='credit').dropna()

# Aggregate to get total time-decay credits for each channel
total_time_decay_credits = credits_df.groupby('variable')['credit'].sum()

total_time_decay_credits
