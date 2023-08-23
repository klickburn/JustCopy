# 5. Active days for credited touchpoints
active_days = credited_df[credited_df['credit'] > 0]['date'].dt.date.value_counts()

# Visualization
plt.figure(figsize=(12, 6))

# Plotting active days for credited touchpoints
active_days.sort_index().plot(kind='line', color='dodgerblue', marker='o')
plt.title('Active Days for Credited Touchpoints')
plt.ylabel('Number of Credited Touchpoints')
plt.xlabel('Date')

plt.tight_layout()
plt.grid(True)
plt.show()
