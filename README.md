# Define the balance bins
bins = list(range(0, 5001, 500)) + [np.inf]
labels = ["$0-$500", "$500-$1000", "$1000-$1500", "$1500-$2000", "$2000-$2500", 
          "$2500-$3000", "$3000-$3500", "$3500-$4000", "$4000-$4500", "$4500-$5000", "$5000+"]

# Categorize account balances
first_channel_df['balance_category'] = pd.cut(first_channel_df['acct_balance'], bins=bins, labels=labels, right=False)

# Generate insights: distribution of the first channel of contact for each balance interval
insights = first_channel_df.groupby(['balance_category', 'channel']).size().unstack().fillna(0).astype(int)
insights
