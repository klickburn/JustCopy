def compute_transition_matrix(df, channel_column):
    """
    Compute the Markov transition matrix based on a given dataframe and channel column.
    """
    # List of unique channels
    channels = df[channel_column].unique()
    
    # Initialize the transition matrix with zeros
    matrix = pd.DataFrame(0, index=channels, columns=channels)
    
    for i in range(len(df) - 1):
        # Check if the next row is from the same account (to avoid counting transitions between different accounts)
        if i + 1 < len(df) and df.iloc[i]['acct_ref_nb'] == df.iloc[i + 1]['acct_ref_nb']:
            current_channel = df.iloc[i][channel_column]
            next_channel = df.iloc[i + 1][channel_column]
            matrix.at[current_channel, next_channel] += 1
            
    # Convert counts to probabilities
    for channel in channels:
        total_transitions = matrix.loc[channel].sum()
        if total_transitions > 0:
            matrix.loc[channel] /= total_transitions
            
    return matrix

# Compute the transition matrix for the dataset
transition_matrix = compute_transition_matrix(modified_events_df, 'channel')
transition_matrix
