# Calculating mean, min, max, and standard deviation of payment amounts, considering only payment amounts above 0

filtered_payment_df = payment_df[payment_df['payment_amt'] > 0]

payment_amount_stats = filtered_payment_df['payment_amt'].agg(['mean', 'min', 'max', 'std'])
payment_amount_stats
