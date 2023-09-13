# Count total events for each channel
total_counts = events_df['channel'].value_counts()

# Add the total counts to the result dataframe
result_df['Total Count'] = total_counts

result_df
