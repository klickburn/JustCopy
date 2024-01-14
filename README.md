# Calculate the percentage of accounts in each segment in jan_df
segment_counts = jan_df['segment'].value_counts(normalize=True) * 100
segment_percentages = segment_counts.to_dict()
segment_percentages
