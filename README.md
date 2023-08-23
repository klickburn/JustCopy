# Identifying rows where the desirable action (PYMT) occurs
pymt_rows = modified_events_df[modified_events_df['channel'] == 'PYMT']

# Dictionary to store the first touchpoint for each account before a payment
first_touchpoints = {}

for _, row in pymt_rows.iterrows():
    acct = row['acct_ref_nb']
    pymt_date = row['date']
    
    # Filter rows for the current account up to the payment date
    previous_rows = modified_events_df[(modified_events_df['acct_ref_nb'] == acct) & 
                                       (modified_events_df['date'] < pymt_date)]
    
    # If there are previous rows, get the first channel
    if not previous_rows.empty:
        first_channel = previous_rows.iloc[0]['channel']
        first_touchpoints[acct] = first_channel

# Convert the dictionary to a DataFrame
first_touchpoints_df = pd.DataFrame(list(first_touchpoints.items()), columns=["acct_ref_nb", "first_touchpoint"])

first_touchpoints_df.head()
