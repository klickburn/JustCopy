# Extract day of week from date
events_df_filtered['day_of_week'] = events_df_filtered['date'].dt.day_name()

# Count the number of payments made on each day of the week
payments_per_day = events_df_filtered[events_df_filtered['channel'] == 'PYMT']['day_of_week'].value_counts()

# Order by days of the week
days_order = ['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday', 'Sunday']
payments_per_day = payments_per_day.re
