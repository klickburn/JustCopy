# List of channels to consider for the first channel
channels_to_consider = ['EMAIL', 'E-LETTER', 'OUTBOUND', 'LETTER', 'TEXT', 'INBOUND']

# Filtering the dataframe to only consider rows with the channels in channels_to_consider
filtered_df = events_df[events_df['channel'].isin(channels_to_consider)]

# Finding the first occurrence of these channels for each account
first_channel_df = filtered_df.groupby('acct_ref_nb').first().reset_index()

# Count of the first channels
first_channel_count = first_channel_df['channel'].value_counts()

first_channel_count
