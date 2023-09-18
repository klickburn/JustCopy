# Channels of interest
channels_of_interest = ['EMAIL', 'E-LETTER', 'OUTBOUND', 'LETTER', 'TEXT']

# Filter by channels of interest
filtered_df = events_df[events_df['channel'].isin(channels_of_interest)]

# Group by account number and get the first channel of contact for each account
first_channels_df = filtered_df.groupby('acct_ref_nb').first().reset_index()

# Merge with the original dataframe to get the customer journey
merged_df = first_channels_df.merge(events_df, on='acct_ref_nb', suffixes=('_first', ''))

# Filter where the date of the first channel is less than the subsequent dates and the subsequent channel is 'PYMT'
success_df = merged_df[(merged_df['date_first'] < merged_df['date']) & (merged_df['channel'] == 'PYMT')]

# Group by segment and first channel and count the number of successes
effectiveness_df = success_df.groupby(['segment', 'channel_first']).size().unstack(fill_value=0)

effectiveness_df

# Plotting the heatmap
plt.figure(figsize=(10, 6))
sns.heatmap(effectiveness_df, annot=True, cmap='Blues', fmt='g')
plt.title('First Channel Effectiveness Leading to a Payment by Segment')
plt.ylabel('Segment')
plt.xlabel('First Contact Channel')
plt.show()
