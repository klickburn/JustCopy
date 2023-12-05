# Creating a line plot with shaded area for standard deviation

plt.figure(figsize=(10, 6))

# Line plot for mean values
plt.plot(communication_means.index, communication_means.values, marker='o', color='b', label='Mean')

# Adding shaded area for standard deviation
plt.fill_between(communication_means.index, 
                 communication_means.values - communication_stds.values, 
                 communication_means.values + communication_stds.values, 
                 color='skyblue', alpha=0.3)

plt.title('Mean and Standard Deviation of Communication Methods')
plt.xlabel('Communication Method')
plt.ylabel('Average Count per Account')
plt.legend()
plt.grid(axis='y', alpha=0.75)

# Show the plot
plt.show()
