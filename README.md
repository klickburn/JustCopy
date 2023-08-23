# Plot the distribution of U-shaped credits across channels
plt.figure(figsize=(10, 6))
total_u_shaped_credits.sort_values().plot(kind='barh', color='teal')
plt.title('Distribution of U-Shaped Credits Across Channels')
plt.xlabel('U-Shaped Credits')
plt.ylabel('Channel')
plt.grid(axis='x')
plt.tight_layout()
plt.show()
