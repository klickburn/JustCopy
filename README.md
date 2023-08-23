# Classify channels into contact and response channels
response_channels = ['LOGIN', 'PROMISE', 'PYMT_PROG', 'PYMT', 'INBOUND']
contact_channels = [channel for channel in events_df['channel'].unique() if channel not in response_channels]

# Filter out the response channels from the dataset
contact_events_df = events_df[events_df['channel'].isin(contact_channels)]

# Count the number of times each contact channel precedes a response channel for each customer
def count_preceding_channels(events_subset):
    return events_subset['channel'].value_counts()

preceding_channel_counts = events_df[events_df['acct_ref_nb'].isin(contact_events_df['acct_ref_nb'])].groupby('acct_ref_nb').apply(count_preceding_channels).unstack().fillna(0)

# Sum across all accounts for each channel
channel_engagement_frequency = preceding_channel_counts.sum()

# Calculate the number of contact channels each customer interacts with before a response
num_channels_before_response = preceding_channel_counts.apply(lambda x: (x > 0).sum(), axis=1)

channel_engagement_frequency, num_channels_before_response.value_counts()
