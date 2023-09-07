# Insight 4: Calculate the ratio of payments to contacts for each unique cluster
# This information is already calculated in most_effective_cluster DataFrame.
# Let's sort it again by the ratio and display the top 10 clusters based on this ratio.

most_effective_cluster.sort_values('payment_to_contact_ratio', ascending=False).head(10)
