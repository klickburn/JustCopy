# Convert the date to month-year format for time series analysis
events_df['date_month_year'] = events_df['date'].dt.to_period('M')

# Plot the frequency of contacts over time by channel
plt.figure(figsize=(16, 10))
sns.countplot(x='date_month_year', hue='channel', data=events_df, palette='viridis')
plt.title('Frequency of Contact Over Time by Channel')
plt.xlabel('Month-Year')
plt.ylabel('Frequency')
plt.legend(title='Channel', loc='upper left')
plt.xticks(rotation=45)
plt.show()
