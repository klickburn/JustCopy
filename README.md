# Counting the number of occurrences of each first touchpoint channel
channel_counts = first_touchpoints_df['first_touchpoint'].value_counts()

# Plotting the distribution of credit among channels
plt.figure(figsize=(12, 7))
channel_counts.plot(kind='bar', color='skyblue', edgecolor='black')
plt.title('Distribution of Credit Among Channels')
plt.xlabel('Channel')
plt.ylabel('Number of Credits')
plt.xticks(rotation=45)
plt.grid(axis='y')

plt.show()
