# Calculate the average time-decay credit per channel
average_time_decay_credit = credits_df.groupby('variable')['credit'].mean()

# Plot the average time-decay credit for each channel
plt.figure(figsize=(10, 6))
average_time_decay_credit.sort_values().plot(kind='barh', color='turquoise')
plt.title('Average Time-Decay Credit per Channel')
plt.xlabel('Average Time-Decay Credit')
plt.ylabel('Channel')
plt.grid(axis='x')
plt.tight_layout()
plt.show()
