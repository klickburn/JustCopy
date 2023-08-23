# Extract month-year from date for aggregation
events_df_filtered['month_year'] = events_df_filtered['date'].dt.to_period('M')

# Calculate the number of unique accounts contacted per month per channel
total_accounts_monthly = events_df_filtered.groupby(['month_year', 'first_contact_channel'])['acct_ref_nb'].nunique().reset_index()

# Calculate the number of unique accounts that made a payment per month per channel
payment_accounts_monthly = events_df_filtered[events_df_filtered['channel'] == 'PYMT'].groupby(['month_year', 'first_contact_channel'])['acct_ref_nb'].nunique().reset_index()

# Avoid the issue by not using fillna directly on the Period column
# Instead, we'll compute success rates and handle missing values during the computation

# Merge the dataframes with 'outer' join to ensure all combinations of month_year and channels are present
merged_monthly = pd.merge(total_accounts_monthly, payment_accounts_monthly, on=['month_year', 'first_contact_channel'], how='outer', suffixes=('_total', '_payment'))
merged_monthly.fillna({'acct_ref_nb_total': 0, 'acct_ref_nb_payment': 0}, inplace=True)

# Calculate monthly success rate
merged_monthly['success_rate'] = merged_monthly['acct_ref_nb_payment'] / merged_monthly['acct_ref_nb_total']

# Pivot the dataframe for visualization
pivot_monthly_success = merged_monthly.pivot(index='month_year', columns='first_contact_channel', values='success_rate')

# Plot the time evolution of success rates
plt.figure(figsize=(14, 8))
pivot_monthly_success.plot(ax=plt.gca())
plt.title('Time Evolution of Success Rate of First Contacts Leading to Payments')
plt.xlabel('Month-Year')
plt.ylabel('Success Rate')
plt.legend(title='Channel', bbox_to_anchor=(1.05, 1), loc='upper left')
plt.grid(axis='y')
plt.tight_layout()
plt.show()
