# Modifying the analysis to display the charged-off amount in millions

# Converting the total charged-off amount to millions
segment_grouped['Total Charged-Off Amount'] = segment_grouped['Total Charged-Off Amount'] / 1e6  # Dividing by 1 million

segment_grouped
