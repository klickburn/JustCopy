# Defining the channels of interest
channels_of_interest = ['EMAIL', 'E-LETTER', 'OUTBOUND', 'LETTER', 'TEXT']

# Filtering by channels of interest
filtered_df_bucket = events_df[events_df['channel'].isin(channels_of_interest)]

# Group by account number and get the first channel of contact for each account
first_channels_bucket_df = filtered_df_bucket.groupby('acct_ref_nb').first().reset_index()

# Merge with the original dataframe to get the customer journey
merged_bucket_df = first_channels_bucket_df.merge(events_df, on='acct_ref_nb', suffixes=('_first', ''))

# Filter where the date of the first channel is less than the subsequent dates and the subsequent channel is 'PYMT'
success_bucket_df = merged_bucket_df[(merged_bucket_df['date_first'] < merged_bucket_df['date']) & (merged_bucket_df['channel'] == 'PYMT')]
success_single_pymt_bucket_df = success_bucket_df.groupby(['acct_ref_nb', 'bucket', 'channel_first']).first().reset_index()

# Group by bucket and first channel and count the number of successes
effectiveness_bucket_df = success_single_pymt_bucket_df.groupby(['bucket', 'channel_first']).size().unstack(fill_value=0)

# Get the total number of times each channel was the first point of contact for each bucket
total_first_contacts_bucket = first_channels_bucket_df.groupby('bucket')['channel'].value_counts().unstack(fill_value=0)

# Calculate the percentage effectiveness considering only a single payment per customer
percentage_effectiveness_bucket = (effectiveness_bucket_df / total_first_contacts_bucket) * 100

percentage_effectiveness_bucket.fillna(0, inplace=True)  # fill any NaN values with 0
percentage_effectiveness_bucket


# Plotting the percentage effectiveness heatmap grouped by bucket
plt.figure(figsize=(12, 7))
sns.heatmap(percentage_effectiveness_bucket, annot=True, cmap='Blues', fmt='.2f')
plt.title('First Channel Effectiveness (in %) Leading to a Payment by Bucket')
plt.ylabel('Bucket')
plt.xlabel('First Contact Channel')
plt.show()
