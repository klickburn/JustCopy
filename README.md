# Recalculate the average number of channels contacted before a payment is made (excluding PYMT)
average_channels_updated = channels_and_dates['channel'].apply(len).mean()

# Plot the updated average number of channels
plt.figure(figsize=(8, 6))
plt.bar(['Average Channels Before Payment'], [average_channels_updated], color='orchid')
plt.title('Updated Average Number of Channels Contacted Before a Payment (Excluding PYMT)')
plt.ylabel('Average Number of Channels')
plt.grid(axis='y', which='both')
plt.tight_layout()
plt.show()

average_channels_updated
