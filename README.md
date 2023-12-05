import pandas as pd

# Load the data
calls_df = pd.read_csv('CALLS_23.csv')
chargeoffs_df = pd.read_csv('CHARGEOFFS_SCORE_23.csv')
comms_df = pd.read_csv('COMMS_CYCLE_23.csv')
payment_df = pd.read_csv('PAYMENT_23.csv')
pmt_program_df = pd.read_csv('PMT_PROGRAM_23.csv')

# Convert Dlq_Date to DateTime format
chargeoffs_df['Dlq_Date'] = pd.to_datetime(chargeoffs_df['Dlq_Date'])

# Filter CHARGEOFFS_SCORE_23 to keep only the dates between 2023-01-31 and 2023-04-30
start_date = '2023-01-31'
end_date = '2023-04-30'
chargeoffs_df = chargeoffs_df[(chargeoffs_df['Dlq_Date'] >= start_date) & (chargeoffs_df['Dlq_Date'] <= end_date)]

# Get the list of ACCT_REF_NB to be removed
removed_acct_numbers = chargeoffs_df['ACCT_REF_NB']

# Remove these accounts from other data frames
calls_df = calls_df[~calls_df['ACCT_REF_NB'].isin(removed_acct_numbers)]
comms_df = comms_df[~comms_df['acct_ref_nb'].isin(removed_acct_numbers)]
payment_df = payment_df[~payment_df['ACCT_REF_NB'].isin(removed_acct_numbers)]
pmt_program_df = pmt_program_df[~pmt_program_df['ACCT_REF_NB'].isin(removed_acct_numbers)]

# The data frames are now filtered as required
