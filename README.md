# Count unique customers for each channel
unique_counts = events_df.groupby('channel')['acct_ref_nb'].nunique()

# Calculate the percentage for each channel
total_unique_customers = events_df['acct_ref_nb'].nunique()
percentages = (unique_counts / total_unique_customers) * 100

# Combining results into a dataframe
result_df = pd.DataFrame({
    'Unique Customers': unique_counts,
    'Percentage': percentages
})

result_df
