# Create a comparison dataframe with U-shaped credits
comparison_df_u_shaped = pd.DataFrame({
    'Total Contacts': total_contacts.drop('PYMT', errors='ignore'),
    'Equal Credit': channel_credits,
    'Time-Decay Credit': total_time_decay_credits,
    'U-Shaped Credit': total_u_shaped_credits
})

# Plot the comparison for each channel
comparison_df_u_shaped.plot(kind='bar', figsize=(16, 8), color=['royalblue', 'mediumseagreen', 'lightcoral', 'teal'])
plt.title('Comparison: Total Contacts vs. Different Attribution Models for Each Channel')
plt.xlabel('Channel')
plt.ylabel('Count')
plt.grid(axis='y')
plt.tight_layout()
plt.show()
