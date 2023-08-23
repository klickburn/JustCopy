# Add date information to the credits dataframe
credits_with_date = credits_df.merge(events_df_filtered[['acct_ref_nb', 'date', 'channel']], left_on=['acct_ref_nb', 'variable'], right_on=['acct_ref_nb', 'channel'])

# Group by date and channel to get the sum of time-decay credits for each channel over time
credits_over_time = credits_with_date.groupby(['date', 'variable'])['credit'].sum().unstack().fillna(0).cumsum()

# Plot the evolution of time-decay credits over time for each channel
plt.figure(figsize=(14, 8))
credits_over_time.plot(ax=plt.gca())
plt.title('Evolution of Time-Decay Credits for Each Channel Over Time')
plt.xlabel('Date')
plt.ylabel('Cumulative Time-Decay Credits')
plt.legend(title='Channel', bbox_to_anchor=(1.05, 1), loc='upper left')
plt.grid(axis='y')
plt.tight_layout()
plt.show()
