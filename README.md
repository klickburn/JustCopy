from collections import Counter

# Function to create a Bag of Words model
def create_bag_of_words(text_list):
    # Join all the text items into a single string
    all_text = ' '.join(text_list)
    # Tokenize the text
    tokens = all_text.split()
    # Create a frequency distribution
    frequency_dist = Counter(tokens)
    return frequency_dist

# Applying Bag of Words model to 'Root_Cause_Processed' and 'Resolution_Detail_Processed'
root_cause_bow = create_bag_of_words(df['Root_Cause_Processed'])
resolution_detail_bow = create_bag_of_words(df['Resolution_Detail_Processed'])

# Displaying the most common words in 'Root_Cause'
print("Most common words in Root Cause:")
print(root_cause_bow.most_common(10))

# Displaying the most common words in 'Resolution_Detail'
print("\nMost common words in Resolution Detail:")
print(resolution_detail_bow.most_common(10))

