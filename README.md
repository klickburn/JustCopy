# Calculate the average account balance for contacts resulting in payment ("PYMT")
average_balance_for_payment = events_df[events_df['channel'] == 'PYMT']['acct_balance'].mean()

average_balance_for_payment
