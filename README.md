
# Merging payment data with charge-offs data to include segment information
merged_payment_segment = pd.merge(payment_df, chargeoffs_df[['ACCT_REF_NB', 'Segment']], on='ACCT_REF_NB')

# Grouping by Segment and calculating total payments received
total_payments_by_segment = merged_payment_segment.groupby('Segment')['payment_amt'].sum()

total_payments_by_segment
