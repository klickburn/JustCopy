# Sorting the dataframe based on the provided order
sort_order = ['EMAIL', 'OUTBOUND', 'TEXT', 'E-LETTER', 'LETTER']
df_channels_sorted = df_channels.set_index('channel').loc[sort_order].reset_index()

df_channels_sorted
