# Calculate the weighted average time to payment for each channel based on the U-shaped credits
weighted_time_to_payment_u_shaped = u_shaped_credits_df.merge(time_to_payment, left_on='acct_ref_nb', right_index=True)
weighted_avg_time_u_shaped = (weighted_time_to_payment_u_shaped.groupby('variable').apply(lambda x: (x['credit'] * x['days_to_payment']).sum()) / weighted_time_to_payment_u_shaped.groupby('variable')['credit'].sum())

# Plot the weighted average time to payment for each channel based on U-shaped credits
plt.figure(figsize=(10, 6))
weighted_avg_time_u_shaped.sort_values().plot(kind='barh', color='gold')
plt.title('Weighted Average Time from First Contact to Payment (U-Shaped Credit)')
plt.xlabel('Average Days to Payment')
plt.ylabel('Channel')
plt.grid(axis='x')
plt.tight_layout()
plt.show()
