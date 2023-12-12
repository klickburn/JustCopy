# Filtering the chargeoffs_df to include only accounts with a balance less than 100,000

filtered_chargeoffs_df = chargeoffs_df[chargeoffs_df['CHRGF_BAL_AM'] < 100000]

# Creating a histogram to visualize the distribution of charge-off balance amounts for these accounts
plt.figure(figsize=(10, 6))
plt.hist(filtered_chargeoffs_df['CHRGF_BAL_AM'], bins=20, color='skyblue', edgecolor='black')
plt.title('Distribution of Charge-Off Balance Amounts (Under 100,000)')
plt.xlabel('Charge-Off Balance Amount')
plt.ylabel('Frequency')
plt.grid(axis='y', alpha=0.75)

# Show the plot
plt.show()
