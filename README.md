# Filter the dataframe for the specified channels
valid_channels = ["EMAIL", "E-LETTER", "OUTBOUND", "LETTER", "TEXT", "INBOUND"]
filtered_df = events_df[events_df['channel'].isin(valid_channels)]

# Group by account and get the first valid channel by date
first_channel_df = filtered_df.groupby('acct_ref_nb').first().reset_index()

first_channel_df.head(10)
