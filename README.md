# Function to assign linear credit to touchpoints before payment
def assign_linear_credit(group):
    payment_idx = group[group['channel'] == 'pymt'].index
    if len(payment_idx) > 0:
        touchpoints_before_payment = group.loc[:payment_idx[0] - 1]
        if not touchpoints_before_payment.empty:
            credit = 1 / len(touchpoints_before_payment)
            group.loc[:payment_idx[0] - 1, 'credit'] = credit
    return group

# Initialize a 'credit' column
modified_events_df['credit'] = 0

# Apply the function to assign linear credit
credited_df = modified_events_df.groupby('acct_ref_nb').apply(assign_linear_credit)

# 1. Total credit for each channel
channel_credit = credited_df.groupby('channel')['credit'].sum()

# 2. Average number of touchpoints before payment
avg_touchpoints = credited_df[credited_df['credit'] > 0].groupby('acct_ref_nb').size().mean()

# 3. Time distribution of credited touchpoints
credited_time_distribution = credited_df[credited_df['credit'] > 0].groupby(credited_df['date'].dt.weekday).size()

# 4. Efficiency of channels
total_channel_counts = credited_df['channel'].value_counts()
credited_channel_counts = credited_df[credited_df['credit'] > 0]['channel'].value_counts()
channel_efficiency_linear = (credited_channel_counts / total_channel_counts).fillna(0)

# Visualization
plt.figure(figsize=(20, 15))

# Plotting total credit for each channel
plt.subplot(2, 2, 1)
channel_credit.sort_values().plot(kind='barh', color='teal')
plt.title('Total Credit for Each Channel')
plt.xlabel('Total Credit')
plt.ylabel('Channel')

# Plotting time distribution of credited touchpoints
plt.subplot(2, 2, 2)
credited_time_distribution.plot(kind='bar', color='gold')
plt.title('Time Distribution of Credited Touchpoints')
plt.ylabel('Number of Credited Touchpoints')
plt.xlabel('Weekday (0=Monday)')

# Plotting efficiency of channels based on linear credit
plt.subplot(2, 2, 3)
channel_efficiency_linear.sort_values().plot(kind='barh', color='plum')
plt.title('Efficiency of Channels (Linear Attribution)')
plt.xlabel('Efficiency Ratio (Credited/Total Touchpoints)')
plt.ylabel('Channel')

plt.tight_layout()
plt.show()

avg_touchpoints
