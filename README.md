# Get the total number of times each channel was the first point of contact for each segment
total_first_contacts = first_channels_df.groupby('segment')['channel'].value_counts().unstack(fill_value=0)

# Calculate the percentage effectiveness
percentage_effectiveness = (effectiveness_df / total_first_contacts) * 100

percentage_effectiveness.fillna(0, inplace=True)  # fill any NaN values with 0
percentage_effectiveness

# Plotting the percentage effectiveness heatmap
plt.figure(figsize=(10, 6))
sns.heatmap(percentage_effectiveness, annot=True, cmap='Blues', fmt='.2f')
plt.title('First Channel Effectiveness (in %) Leading to a Payment by Segment')
plt.ylabel('Segment')
plt.xlabel('First Contact Channel')
plt.show()
