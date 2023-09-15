def calculate_metrics(events_df):
    # 1. Volume
    volume = events_df['channel'].value_counts()

    # 2. Unique Acct Volume
    unique_acct_volume = events_df.groupby('channel')['acct_ref_nb'].nunique()

    # 3. Activity Per Acct
    activity_per_acct = volume / unique_acct_volume

    # 4. Channel Mix %
    total_contacts = len(events_df)
    channel_mix_percent = (volume / total_contacts) * 100

    # 5. Payments
    payments = events_df[events_df['channel'] == 'PYMT'].groupby('acct_ref_nb')['date'].min().reset_index()
    channel_before_payment = events_df.merge(payments, on='acct_ref_nb', how='inner', suffixes=('', '_pymt'))
    payments_count = channel_before_payment[channel_before_payment['date'] < channel_before_payment['date_pymt']].groupby('channel').size()

    # 6. Promise
    promise = events_df[events_df['channel'] == 'PROMISE'].groupby('acct_ref_nb')['date'].min().reset_index()
    channel_before_promise = events_df.merge(promise, on='acct_ref_nb', how='inner', suffixes=('', '_promise'))
    promise_count = channel_before_promise[channel_before_promise['date'] < channel_before_promise['date_promise']].groupby('channel').size()

    # 7. Pymt_program
    pymt_program = events_df[events_df['channel'] == 'PYMT_PROGRAM'].groupby('acct_ref_nb')['date'].min().reset_index()
    channel_before_pymt_program = events_df.merge(pymt_program, on='acct_ref_nb', how='inner', suffixes=('', '_pymt_program'))
    pymt_program_count = channel_before_pymt_program[channel_before_pymt_program['date'] < channel_before_pymt_program['date_pymt_program']].groupby('channel').size()

    # 8. % of Accounts
    total_unique_accounts = events_df['acct_ref_nb'].nunique()
    percent_of_accounts = (unique_acct_volume / total_unique_accounts) * 100

    # Initializing the metrics dataframe
    metrics_df = pd.DataFrame({
        'Channel': volume.index,
        'Volume': volume.values,
        'Unique Acct Volume': unique_acct_volume.values,
        'Activity Per Acct': activity_per_acct.values,
        'Channel Mix %': channel_mix_percent.values,
        'Payments': payments_count,
        'Promise': promise_count,
        'PYMT_PROGRAM': pymt_program_count,
        '% Of Accounts': percent_of_accounts.values
    })
    
    # 9. %pay in 30 days
    payment_dates = events_df[events_df['channel'] == 'PYMT']
    pay_in_30_days_dict = {}
    for channel in metrics_df['Channel']:
        channel_df = events_df[events_df['channel'] == channel]
        merged_df = channel_df.merge(payment_dates, on='acct_ref_nb', how='left', suffixes=('', '_pymt'))
        pay_in_30_days = merged_df[(merged_df['date_pymt'] >= merged_df['date']) & 
                                   (merged_df['date_pymt'] <= (merged_df['date'] + timedelta(days=30)))]
        pay_in_30_days_dict[channel] = pay_in_30_days['acct_ref_nb'].nunique()
    metrics_df['%pay in 30 days'] = metrics_df['Channel'].map(pay_in_30_days_dict)

    # Replacing NaN values with 0
    metrics_df.fillna(0, inplace=True)
    
    return metrics_df

# Using the function to calculate metrics for the sample dataframe
calculate_metrics(events_df)
