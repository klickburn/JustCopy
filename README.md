import matplotlib.pyplot as plt

# Removing 'Online' payment type from the data
total_payments_without_online = total_payments_by_type_thousands.drop('Online')

# Creating a bar plot for the total payments in thousands for each payment type (excluding 'Online')
plt.figure(figsize=(8, 6))
total_payments_without_online.plot(kind='bar', color='teal')
plt.title('Total Payments by Payment Type (in Thousands)')
plt.xlabel('Payment Type')
plt.ylabel('Total Payments (in Thousands)')
plt.xticks(rotation=45)
plt.grid(axis='y', alpha=0.75)

# Show the plot
plt.show()
