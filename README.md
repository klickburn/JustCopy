# Remove PYMT from credited channels
if 'PYMT' in channel_credits:
    channel_credits = channel_credits.drop('PYMT')

# Recalculate and plot the distribution of credits across channels
plt.figure(figsize=(10, 6))
channel_credits.sort_values().plot(kind='barh', color='salmon')
plt.title('Distribution of Credits Across Channels (Excluding PYMT)')
plt.xlabel('Credits')
plt.ylabel('Channel')
plt.grid(axis='x')
plt.tight_layout()
plt.show()
