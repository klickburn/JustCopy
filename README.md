# Filter accounts that made a payment
accounts_with_payment = events_df[events_df['channel'] == 'PYMT']['acct_ref_nb'].unique()

# Get the sequence of channels leading up to the payment for each account
conversion_paths = events_df[events_df['acct_ref_nb'].isin(accounts_with_payment)].groupby('acct_ref_nb')['channel'].apply(list)

# Convert the sequences to a string format for easier counting
conversion_paths_str = conversion_paths.apply(lambda x: ' -> '.join(x))

# Count the frequency of each unique sequence
path_frequencies = conversion_paths_str.value_counts()

# Display the top conversion path and its frequency
top_conversion_path = path_frequencies.head(1)
top_conversion_path
