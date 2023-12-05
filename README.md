
# Recalculating the duration between Dlq_Date and CHRGF_CPLT_DT in days for the renamed dataframe
chargeoffs_df['Duration_to_Chargeoff'] = (chargeoffs_df['CHRGF_CPLT_DT'] - chargeoffs_df['Dlq_Date']).dt.days

# Summary statistics of the duration
duration_stats_renamed = chargeoffs_df['Duration_to_Chargeoff'].describe()

duration_stats_renamed
