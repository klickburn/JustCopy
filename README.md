# Calculate the total number of contacts for each channel
total_contacts = events_df_filtered['channel'].value_counts()

# Recreate the comparison dataframe without PYMT
comparison_df_updated = pd.DataFrame({
    'Total Contacts': total_contacts.drop('PYMT', errors='ignore'),
    'Credited Contacts': channel_credits
})

# Plot the updated total contacts vs credited contacts for each channel
comparison_df_updated.plot(kind='bar', figsize=(12, 7), color=['royalblue', 'mediumseagreen'])
plt.title('Updated Total Number of Contacts vs. Credited Contacts for Each Channel (Excluding PYMT)')
plt.xlabel('Channel')
plt.ylabel('Count')
plt.grid(axis='y')
plt.tight_layout()
plt.show()
