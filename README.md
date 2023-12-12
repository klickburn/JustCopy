calls_details_df['channel'] = 'Call'  # Adding a 'channel' column for uniformity

comms_df['channel'] = comms_df['COMMUNICATION_TYPE']  # Adding a 'channel' column

# Combining calls_details_df with comms_df
combined_contacts_df = pd.concat([
    calls_details_df.rename(columns={'calldate': 'contact_date'}),
    comms_df.rename(columns={'comm_dt': 'contact_date'})
])

# Sorting by account and contact date to find the first contact
combined_contacts_df_sorted = combined_contacts_df.sort_values(by=['acct_ref_nb', 'contact_date'])

# Getting the first contact channel for each account
first_contact_channel = combined_contacts_df_sorted.groupby('acct_ref_nb').first()['channel']

first_contact_channel.head()
