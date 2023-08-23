channels = ['email', 'e-letter', 'outbound', 'letter', 'text']

# Sorting the dataframe by account number and date
modified_events_df = modified_events_df.sort_values(by=['acct_ref_nb', 'date'])

# Function to get the touchpoint before payment
def get_touchpoint_before_payment(group):
    for idx, row in group.iterrows():
        if row['channel'] == 'pymt' and idx > 0:
            prev_row = group.loc[idx-1]
            if prev_row['channel'] in channels:
                return prev_row['channel']
    return None

# Applying the function to each group of acct_ref_nb
touchpoints = modified_events_df.groupby('acct_ref_nb').apply(get_touchpoint_before_payment)

# Counting the occurrences of each touchpoint leading to a payment
touchpoint_counts = touchpoints.value_counts()
touchpoint_counts
