import matplotlib.pyplot as plt

# Plotting the distribution of Duration_to_Chargeoff
plt.figure(figsize=(10, 6))
plt.hist(chargeoffs_df['Duration_to_Chargeoff'], bins=range(1, 13), edgecolor='black', color='skyblue')
plt.title('Distribution of Duration from Delinquency to Charge-off')
plt.xlabel('Duration in Days')
plt.ylabel('Frequency')
plt.xticks(range(1, 12))
plt.grid(axis='y', alpha=0.75)

# Show the plot
plt.show()
