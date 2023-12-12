
# Creating a histogram to visualize the distribution of charge-off balance amounts
plt.figure(figsize=(10, 6))
plt.hist(chargeoffs_df['CHRGF_BAL_AM'], bins=20, color='skyblue', edgecolor='black')
plt.title('Distribution of Charge-Off Balance Amounts')
plt.xlabel('Charge-Off Balance Amount')
plt.ylabel('Frequency')
plt.grid(axis='y', alpha=0.75)

# Show the plot
plt.show()
