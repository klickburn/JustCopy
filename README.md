# Plotting the frequency of each channel driving customer engagement
plt.figure(figsize=(10, 6))
channel_engagement_frequency[contact_channels].sort_values().plot(kind='barh', color='orchid')
plt.title('Frequency of Each Channel Driving Customer Engagement')
plt.xlabel('Frequency')
plt.ylabel('Channel')
plt.grid(axis='x')
plt.tight_layout()
plt.show()
