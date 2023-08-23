# Recreate the comparison dataframe with time-decay credits
comparison_df_time_decay = pd.DataFrame({
    'Total Contacts': total_contacts.drop('PYMT', errors='ignore'),
    'Equal Credit': channel_credits,
    'Time-Decay Credit': total_time_decay_credits
})

# Plot the comparison for each channel
comparison_df_time_decay.plot(kind='bar', figsize=(14, 7), color=['royalblue', 'mediumseagreen', 'lightcoral'])
plt.title('Comparison: Total Contacts vs. Equal Credit vs. Time-Decay Credit for Each Channel')
plt.xlabel('Channel')
plt.ylabel('Count')
plt.grid(axis='y')
plt.tight_layout()
plt.show()
