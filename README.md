# Counting the number of accounts in each program type

accounts_per_program = pmt_program_df['program_type'].value_counts()

accounts_per_program


# Merging payment data with program data on account number
merged_payment_program = pd.merge(payment_df, pmt_program_df, on='ACCT_REF_NB')

# Filtering to include only payments made after enrolling in a program
merged_payment_program_filtered = merged_payment_program[merged_payment_program['payment_date'] >= merged_payment_program['pgm_dt']]

# Grouping by program type and calculating total payments received
total_payments_by_program = merged_payment_program_filtered.groupby('program_type')['payment_amt'].sum()

total_payments_by_program
