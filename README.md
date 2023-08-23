def assign_u_shaped_credits(channels_list):
    n = len(channels_list)
    
    # If there's only one channel, assign full credit to it
    if n == 1:
        return {channels_list[0]: 1}
    
    # If there are two channels, assign half credit to each
    elif n == 2:
        return {channel: 0.5 for channel in channels_list}
    
    # For more than two channels, assign 40% credit to first and last touchpoints, and distribute 20% equally among others
    else:
        credit_for_others = 0.2 / (n - 2)
        credits_dict = {channels_list[0]: 0.4, channels_list[-1]: 0.4}
        
        # Assign equal credit to intermediate channels
        for channel in channels_list[1:-1]:
            credits_dict[channel] = credits_dict.get(channel, 0) + credit_for_others
            
        return credits_dict

# Assign U-shaped credits to channels for each account
u_shaped_credits_per_account = channels_before_payment.apply(assign_u_shaped_credits)

# Convert the dictionary of credits to a dataframe for easier aggregation
u_shaped_credits_df = u_shaped_credits_per_account.apply(pd.Series).reset_index().melt(id_vars='acct_ref_nb', value_name='credit').dropna()

# Aggregate to get total U-shaped credits for each channel
total_u_shaped_credits = u_shaped_credits_df.groupby('variable')['credit'].sum()

total_u_shaped_credits
