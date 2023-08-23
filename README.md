# Bar chart for payments attributed to each channel
plt.figure(figsize=(10, 6))
touchpoint_counts.plot(kind='bar', color='skyblue')
plt.title("Number of Payments Attributed to Each Channel")
plt.xlabel("Channel")
plt.ylabel("Number of Payments")
plt.xticks(rotation=45)
plt.tight_layout()
plt.show()

# Pie chart for proportion of payments per channel
plt.figure(figsize=(8, 8))
touchpoint_counts.plot(kind='pie', autopct='%1.1f%%', startangle=140, colors=['lightcoral', 'lightskyblue', 'yellowgreen'])
plt.title("Proportion of Payments per Channel")
plt.ylabel("")
plt.show()
