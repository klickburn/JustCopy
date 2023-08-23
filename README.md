# Order by days of the week
payments_per_day = payments_per_day.reindex(days_order)

# Plot the distribution of payments across days of the week
plt.figure(figsize=(10, 6))
payments_per_day.plot(kind='bar', color='orchid')
plt.title('Number of Payments Made on Each Day of the Week')
plt.xlabel('Day of the Week')
plt.ylabel('Number of Payments')
plt.grid(axis='y')
plt.show()
