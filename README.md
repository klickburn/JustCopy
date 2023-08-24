from nltk import bigrams, FreqDist
from collections import defaultdict

# Extract bigrams from the data
sequences = modified_events_df.groupby('acct_ref_nb')['channel'].apply(list)
all_bigrams = [bigram for seq in sequences for bigram in bigrams(seq)]

# Build a frequency distribution for each bigram
bigram_freq = FreqDist(all_bigrams)

# Build a dictionary to store possible next steps for each bigram
next_steps = defaultdict(list)

for seq in sequences:
    for i in range(len(seq) - 2):  # -2 because we need to look at the next item as well
        current_bigram = (seq[i], seq[i + 1])
        next_channel = seq[i + 2]
        next_steps[current_bigram].append(next_channel)

# For each bigram, determine the most likely next step
most_likely_next = {}
for bigram, next_channels in next_steps.items():
    most_likely_next[bigram] = FreqDist(next_channels).most_common(1)[0][0]

# Example: Predict the next touchpoint for a sequence ('EMAIL', 'LETTER')
sample_bigram = ('EMAIL', 'LETTER')
predicted_touchpoint_ngram = most_likely_next.get(sample_bigram, None)

predicted_touchpoint_ngram
