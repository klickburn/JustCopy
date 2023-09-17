# Extract unique channels from the dataframe (excluding 'PYMT')
channels_to_check = events_df['channel'].unique().tolist()
channels_to_check.remove('PYMT')

# Re-compute the percentages as described earlier
result_dict = {}

for channel in channels_to_check:
    # Extract accounts and dates for the current channel
    channel_data = events_df[events_df['channel'] == channel][['acct_ref_nb', 'date']]
    
    # Merge with the main dataframe to get future events for these accounts
    merged_data = pd.merge(channel_data, events_df, on='acct_ref_nb', suffixes=('_channel', '_event'))
    
    # Filter where the event date is within 30 days of the channel date and the event is 'PYMT'
    payment_within_30_days = merged_data[
        (merged_data['date_event'] > merged_data['date_channel']) &
        (merged_data['date_event'] <= merged_data['date_channel'] + timedelta(days=30)) &
        (merged_data['channel'] == 'PYMT')
    ]
    
    # Count unique accounts that made a payment within 30 days
    accounts_paid = payment_within_30_days['acct_ref_nb'].nunique()
    
    # Count unique accounts contacted via the current channel
    total_accounts_contacted = channel_data['acct_ref_nb'].nunique()
    
    # Calculate the percentage
    percentage_paid = (accounts_paid / total_accounts_contacted) * 100 if total_accounts_contacted > 0 else 0
    
    # Store the result
    result_dict[channel] = percentage_paid

result_dict
