import pandas as pd
import nltk
from nltk.corpus import stopwords
from nltk.stem import PorterStemmer
from nltk.tokenize import word_tokenize
import re

# No need to manually download 'punkt' and 'stopwords' if nltk_data is installed

# Initialize the Porter Stemmer
stemmer = PorterStemmer()

# Function for text preprocessing
def preprocess_text(text):
    # Convert text to lowercase
    text = text.lower()
    # Remove punctuation and special characters
    text = re.sub(r'[^\w\s]', '', text)
    # Tokenize text
    tokens = word_tokenize(text)
    # Remove stop words and apply stemming
    tokens = [stemmer.stem(word) for word in tokens if word not in stopwords.words('english')]
    return ' '.join(tokens)

# Example usage
df['Root_Cause_Processed'] = df['Root_Cause'].apply(preprocess_text)
df['Resolution_Detail_Processed'] = df['Resolution_Detail'].apply(preprocess_text)

# Displaying the processed DataFrame
print(df[['Root_Cause', 'Root_Cause_Processed', 'Resolution_Detail', 'Resolution_Detail_Processed']])
