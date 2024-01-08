import pandas as pd
import re

# Function to remove punctuation and special characters
def remove_punctuation(text):
    return re.sub(r'[^\w\s]', '', text)

# Basic manual stop words list (you can expand this list based on your observations)
stop_words = set(["the", "and", "is", "in", "at", "which", "on", "for", "with", "a", "an", "to", "of"])

# Function for basic text preprocessing
def preprocess_text(text):
    # Convert text to lowercase
    text = text.lower()
    # Remove punctuation
    text = remove_punctuation(text)
    # Tokenize text by splitting on whitespace
    tokens = text.split()
    # Remove stop words
    tokens = [word for word in tokens if word not in stop_words]
    return ' '.join(tokens)

# Example DataFrame
data = {
    'CCOMS_ID': [1, 2, 3],
    'Root_Cause': ["Password reset issue", "Server downtime", "Login failure"],
    'Resolution_Detail': ["Reset by admin", "Rebooted server", "Checked network"]
}
df = pd.DataFrame(data)

# Applying preprocessing to DataFrame
df['Root_Cause_Processed'] = df['Root_Cause'].apply(preprocess_text)
df['Resolution_Detail_Processed'] = df['Resolution_Detail'].apply(preprocess_text)

# Display the processed DataFrame
print(df)
