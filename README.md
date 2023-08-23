# 3. Actions Leading to Payment
def action_before_payment(group):
    actions = []
    for idx, row in group.iterrows():
        if row['channel'] == 'pymt' and idx > 0:
            prev_row = group.loc[idx-1]
            actions.append(prev_row['channel'])
    return actions

actions_before_pymt_list = modified_events_df.groupby('acct_ref_nb').apply(action_before_payment)
actions_before_pymt = pd.Series([item for sublist in actions_before_pymt_list for item in sublist]).value_counts()

# 4. Frequency of Touchpoints per Customer
contact_frequency = modified_events_df[modified_events_df['channel'].isin(channels)].groupby('acct_ref_nb').size()

# Visualization
plt.figure(figsize=(15, 10))

# Plotting actions leading to payment
plt.subplot(2, 2, 1)
actions_before_pymt.plot(kind='bar', color='lightblue')
plt.title('Actions Leading to Payment')
plt.ylabel('Frequency')
plt.xlabel('Action/Channel')
plt.xticks(rotation=45)

# Plotting frequency of touchpoints per customer
plt.subplot(2, 2, 2)
contact_frequency.plot(kind='hist', bins=20, color='lightcoral', edgecolor='black')
plt.title('Frequency of Touchpoints per Customer')
plt.ylabel('Number of Customers')
plt.xlabel('Number of Touchpoints')
plt.xticks(rotation=0)

plt.tight_layout()
plt.show()
