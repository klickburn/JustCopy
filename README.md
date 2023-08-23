# Function to get the time difference between the touchpoint and payment
def time_to_payment(group):
    for idx, row in group.iterrows():
        if row['channel'] == 'pymt' and idx > 0:
            prev_row = group.loc[idx-1]
            if prev_row['channel'] in channels:
                return (row['date'] - prev_row['date']).days
    return None

modified_events_df['date'] = pd.to_datetime(modified_events_df['date'])

# Applying the function to each group of acct_ref_nb
time_diffs = modified_events_df.groupby('acct_ref_nb').apply(time_to_payment)

# Average time between the first touchpoint and the payment for each channel
avg_time_to_payment = time_diffs.groupby(touchpoints).mean()

# Number of actions (interactions) taken by a customer before making a payment
num_actions_before_payment = modified_events_df[modified_events_df['channel'] != 'pymt'].groupby('acct_ref_nb').size()

avg_time_to_payment, num_actions_before_payment.mean()
