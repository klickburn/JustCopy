# Step 2: Filter rows where the current channel is 'PYMT_PROGRAM' and then count occurrences of each previous channel
pymt_program_after_channel = events_df[events_df['channel'] == 'PYMT_PROGRAM']['previous_channel'].value_counts()

# Step 3: Count total volume of PYMT_PROGRAM events
total_pymt_program_volume = events_df[events_df['channel'] == 'PYMT_PROGRAM'].shape[0]

pymt_program_after_channel, total_pymt_program_volume
