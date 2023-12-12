# Counting how many accounts made at least one payment and how many did not make any payment

# Total number of unique accounts in the dataset
total_accounts = payment_df['ACCT_REF_NB'].nunique()

# Number of accounts that made at least one payment
accounts_made_payment = payment_df['ACCT_REF_NB'].nunique()

# Number of accounts that did not make any payment
accounts_did_not_pay = total_accounts - accounts_made_payment

accounts_made_payment, accounts_did_not_pay
