# Calculating the number of accounts and total charge-off amount for accounts with balances below and above 5000

# Accounts with charge-off balance below 5000
below_5000 = chargeoffs_df[chargeoffs_df['CHRGF_BAL_AM'] < 5000]
num_accounts_below_5000 = below_5000['ACCT_REF_NB'].nunique()
total_amount_below_5000 = below_5000['CHRGF_BAL_AM'].sum()

# Accounts with charge-off balance above 5000
above_5000 = chargeoffs_df[chargeoffs_df['CHRGF_BAL_AM'] >= 5000]
num_accounts_above_5000 = above_5000['ACCT_REF_NB'].nunique()
total_amount_above_5000 = above_5000['CHRGF_BAL_AM'].sum()

num_accounts_below_5000, total_amount_below_5000, num_accounts_above_5000, total_amount_above_5000
