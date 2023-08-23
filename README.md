# Calculate the frequency of each channel
channel_frequency = events_df_filtered['channel'].value_counts()

# Plot the frequency of each channel
plt.figure(figsize=(10, 6))
channel_frequency.plot(kind='bar', color='teal')
plt.title('Frequency of Each Channel Across All Accounts')
plt.xlabel('Channel')
plt.ylabel('Frequency')
plt.grid(axis='y')
plt.show()
