# Filter rows where channel is 'PYMT'
payments_df = events_df[events_df['channel'] == 'PYMT']

# Merge with the first_channel_df to get the date of first contact
merged_df = payments_df.merge(first_channel_df[['acct_ref_nb', 'date', 'channel', 'balance_category']],
                              on='acct_ref_nb', 
                              how='left', 
                              suffixes=('_pymt', '_first_contact'))

# Calculate the difference in days between the payment and first contact
merged_df['days_to_pymt'] = (merged_df['date_pymt'] - merged_df['date_first_contact']).dt.days

# Filter out records where the payment was made after more than 30 days
successful_pymts = merged_df[merged_df['days_to_pymt'] <= 30]

# Insights: Count of successful payments for each balance category and channel
effectiveness_insights = successful_pymts.groupby(['balance_category', 'channel_first_contact']).size().unstack().fillna(0).astype(int)
effectiveness_insights

# Plotting bar chart for effectiveness insights
ax = effectiveness_insights.plot(kind='bar', figsize=(15, 10), colormap="viridis", width=0.8)

# Setting titles, labels, and legend
plt.title("Effectiveness of Each Channel by Balance Category (Payments within 30 days)", fontsize=16)
plt.xlabel("Balance Category", fontsize=14)
plt.ylabel("Number of Successful Payments", fontsize=14)
plt.xticks(rotation=45)
plt.legend(title="First Contact Channel", fontsize=12, title_fontsize=12)

plt.tight_layout()
plt.show()
