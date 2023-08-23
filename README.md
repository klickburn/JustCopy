# Plot the distribution of time-decay credits across channels
plt.figure(figsize=(10, 6))
total_time_decay_credits.sort_values().plot(kind='barh', color='skyblue')
plt.title('Distribution of Time-Decay Credits Across Channels')
plt.xlabel('Time-Decay Credits')
plt.ylabel('Channel')
plt.grid(axis='x')
plt.tight_layout()
plt.show()
