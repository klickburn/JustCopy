# Plot the monthly trend of contacts for different account balance ranges
plt.figure(figsize=(16, 10))
sns.countplot(x='date_month_year', hue='balance_range', data=events_df, palette='viridis')
plt.title('Monthly Trend of Contacts for Different Account Balance Ranges')
plt.xlabel('Month-Year')
plt.ylabel('Frequency')
plt.legend(title='Account Balance Range ($)', loc='upper left')
plt.xticks(rotation=45)
plt.show()
