plt.figure(figsize=(15, 10))

# Plotting the data
sns.heatmap(insights, annot=True, cmap="YlGnBu", cbar=True, linewidths=0.5)

# Setting titles and labels
plt.title("Distribution of First Channel of Contact by Balance Category", fontsize=16)
plt.xlabel("Channel", fontsize=14)
plt.ylabel("Balance Category", fontsize=14)
plt.xticks(rotation=45)
plt.tight_layout()

plt.show()
