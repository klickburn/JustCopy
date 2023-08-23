# 1. Distribution of Actions
action_distribution = modified_events_df['channel'].value_counts()

# 2. Time Distribution
time_distribution = modified_events_df.groupby(modified_events_df['date'].dt.weekday)['channel'].count()

# Visualization
plt.figure(figsize=(15, 10))

# Plotting distribution of actions
plt.subplot(2, 2, 1)
action_distribution.plot(kind='bar', color='mediumaquamarine')
plt.title('Distribution of Actions')
plt.ylabel('Frequency')
plt.xlabel('Action/Channel')
plt.xticks(rotation=45)

# Plotting time distribution (by weekday)
plt.subplot(2, 2, 2)
time_distribution.plot(kind='bar', color='lightsalmon')
plt.title('Action Distribution by Weekday')
plt.ylabel('Number of Actions')
plt.xlabel('Weekday (0=Monday)')
plt.xticks(rotation=0)

plt.tight_layout()
plt.show()
