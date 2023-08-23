# Plot the weighted average time to payment for each channel
plt.figure(figsize=(10, 6))
weighted_avg_time.sort_values().plot(kind='barh', color='gold')
plt.title('Weighted Average Time from First Contact to Payment (Time-Decay Credit)')
plt.xlabel('Average Days to Payment')
plt.ylabel('Channel')
plt.grid(axis='x')
plt.tight_layout()
plt.show()
