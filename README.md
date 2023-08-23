# Group by date and channel to get the sum of U-shaped credits for each channel over time
u_shaped_credits_over_time = u_shaped_credits_df.merge(events_df_filtered[['acct_ref_nb', 'date', 'channel']], left_on=['acct_ref_nb', 'variable'], right_on=['acct_ref_nb', 'channel'])
u_shaped_credits_over_time = u_shaped_credits_over_time.groupby(['date', 'variable'])['credit'].sum().unstack().fillna(0).cumsum()

# Plot the evolution of U-shaped credits over time for each channel
plt.figure(figsize=(14, 8))
u_shaped_credits_over_time.plot(ax=plt.gca())
plt.title('Evolution of U-Shaped Credits for Each Channel Over Time')
plt.xlabel('Date')
plt.ylabel('Cumulative U-Shaped Credits')
plt.legend(title='Channel', bbox_to_anchor=(1.05, 1), loc='upper left')
plt.grid(axis='y')
plt.tight_layout()
plt.show()
