# Categorize account balances into different ranges for easier analysis
bins = [0, 100, 200, 300, 400, 500, 600, 700, 800, 900, 1000]
labels = ['0-100', '101-200', '201-300', '301-400', '401-500', '501-600', '601-700', '701-800', '801-900', '901-1000']
events_df['balance_range'] = pd.cut(events_df['acct_balance'], bins=bins, labels=labels, right=False)

# Calculate the total number of contacts for each channel within each account balance range
total_contacts_per_channel_per_balance_range = events_df.groupby(['balance_range', 'channel']).size().reset_index(name='total_contacts')

# Display the data
total_contacts_per_channel_per_balance_range.head(10)
