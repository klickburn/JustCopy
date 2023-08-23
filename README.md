# For accounts with payments, get the list of channels contacted and the last date of contact before payment
channels_and_dates = events_df_filtered[events_df_filtered['acct_ref_nb'].isin(accounts_with_payment)].groupby('acct_ref_nb').agg({
    'channel': lambda x: [channel for channel in x.unique() if channel != 'PYMT' or list(x).count('PYMT') == 1],
    'date': 'max'
}).reset_index()

# Remove PYMT from the channels contacted before payment
channels_and_dates['channel'] = channels_and_dates['channel'].apply(lambda x: [channel for channel in x if channel != 'PYMT'])

# Explode the dataframe to assign equal credit to each channel for each account
credits_over_time_updated = channels_and_dates.explode('channel')

# Group by date and channel to get the number of credits for each channel over time
credits_over_time_updated = credits_over_time_updated.groupby(['date', 'channel']).size().unstack().fillna(0).cumsum()

# Plot the updated evolution of credits over time for each channel
plt.figure(figsize=(14, 8))
credits_over_time_updated.plot(ax=plt.gca())
plt.title('Updated Evolution of Credits for Each Channel Over Time (Excluding PYMT)')
plt.xlabel('Date')
plt.ylabel('Cumulative Credits')
plt.legend(title='Channel', bbox_to_anchor=(1.05, 1), loc='upper left')
plt.grid(axis='y')
plt.tight_layout()
plt.show()
