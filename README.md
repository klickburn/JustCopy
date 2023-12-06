# Calculating total payments for each payment type
total_payments_by_type = payment_df.groupby('payment_type')['payment_amt'].sum()

total_payments_by_type
