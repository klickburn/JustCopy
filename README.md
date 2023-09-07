# Calculate the effectiveness of each channel within each bucket in terms of the ratio of contacts to payments
# Note: Since all payments are directly attributed to the "PYMT" channel in this sample data,
# we can only calculate the effectiveness for the "PYMT" channel within each bucket.

# Filter out only the "PYMT" channel data
pymt_data = events_df[events_df['channel'] == 'PYMT']

# Calculate the total number of "PYMT" contacts within each bucket
total_pymt_per_bucket = pymt_data.groupby('bucket').size().reset_index(name='total_pymt_contacts')

# Calculate the ratio of "PYMT" contacts to total contacts within each bucket
channel_effectiveness_per_bucket = pd.merge(total_pymt_per_bucket, total_contacts_per_bucket, on='bucket')
channel_effectiveness_per_bucket['pymt_to_contact_ratio'] = channel_effectiveness_per_bucket['total_pymt_contacts'] / channel_effectiveness_per_bucket['total_contacts']

# Display the data
channel_effectiveness_per_bucket
