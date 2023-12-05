import seaborn as sns

# Setting up the matplotlib figure
plt.figure(figsize=(14, 6))

# Creating subplots
plt.subplot(1, 2, 1)
sns.boxplot(x=chargeoffs_df['Duration_to_Chargeoff'], color='lightgreen')
plt.title('Box Plot of Duration from Delinquency to Charge-off')
plt.xlabel('Duration in Days')

plt.subplot(1, 2, 2)
sns.kdeplot(chargeoffs_df['Duration_to_Chargeoff'], shade=True, color='lightblue')
plt.title('Density Plot of Duration from Delinquency to Charge-off')
plt.xlabel('Duration in Days')

# Show the plots
plt.tight_layout()
plt.show()
