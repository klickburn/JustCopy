# Calculate the updated average U-shaped credit per channel
average_u_shaped_credit = u_shaped_credits_df.groupby('variable')['credit'].mean()

# Plot the updated average U-shaped credit for each channel
plt.figure(figsize=(10, 6))
average_u_shaped_credit.sort_values().plot(kind='barh', color='turquoise')
plt.title('Average U-Shaped Credit per Channel')
plt.xlabel('Average U-Shaped Credit')
plt.ylabel('Channel')
plt.grid(axis='x')
plt.tight_layout()
plt.show()
