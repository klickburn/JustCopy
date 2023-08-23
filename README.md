# Calculate the number of payment events per month
payments_per_month = events_df_filtered[events_df_filtered['channel'] == 'PYMT']['month_year'].value_counts().sort_index()

# Plot the distribution of payment events across the year
plt.figure(figsize=(14, 6))
payments_per_month.plot(kind='bar', color='slateblue')
plt.title('Distribution of Payment Events Across the Year')
plt.xlabel('Month-Year')
plt.ylabel('Number of Payments')
plt.grid(axis='y')
plt.tight_layout()
plt.show()
