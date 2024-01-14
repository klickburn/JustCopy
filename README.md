# Replace non-numeric values with '1' in the bucket column
jan_df['bucket'] = pd.to_numeric(jan_df['bucket'], errors='coerce').fillna(1).astype(int)

jan_df.head()  # Display the first few rows to confirm the change
