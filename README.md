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
