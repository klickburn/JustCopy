# For each customer, after the first contact, consider only the first occurrence of 'PYMT'
success_single_pymt_df = merged_df[(merged_df['date_first'] < merged_df['date']) & (merged_df['channel'] == 'PYMT')]
success_single_pymt_df = success_single_pymt_df.groupby(['acct_ref_nb', 'segment', 'channel_first']).first().reset_index()

# Group by segment and first channel and count the number of successes
effectiveness_single_pymt_df = success_single_pymt_df.groupby(['segment', 'channel_first']).size().unstack(fill_value=0)

# Calculate the percentage effectiveness considering only a single payment per customer
percentage_effectiveness_single_pymt = (effectiveness_single_pymt_df / total_first_contacts) * 100

percentage_effectiveness_single_pymt.fillna(0, inplace=True)  # fill any NaN values with 0
percentage_effectiveness_single_pymt

# Plotting the corrected percentage effectiveness heatmap
plt.figure(figsize=(10, 6))
sns.heatmap(percentage_effectiveness_single_pymt, annot=True, cmap='Blues', fmt='.2f')
plt.title('Corrected First Channel Effectiveness (in %) Leading to a Payment by Segment')
plt.ylabel('Segment')
plt.xlabel('First Contact Channel')
plt.show()
