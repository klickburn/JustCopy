import gensim
from gensim import corpora
from gensim.models.ldamodel import LdaModel

# Function to create a term dictionary and corpus, then apply LDA model
def perform_lda(text_data):
    # Create a term dictionary of our corpus, where every unique term is assigned an index
    dictionary = corpora.Dictionary(text_data)

    # Convert list of documents (corpus) into Document Term Matrix using the dictionary
    doc_term_matrix = [dictionary.doc2bow(doc) for doc in text_data]

    # Create the LDA model
    lda_model = LdaModel(doc_term_matrix, num_topics=3, id2word=dictionary, passes=15, random_state=42)

    return lda_model, dictionary

# Applying LDA to each column
for column in ['Issue Description', 'Root Cause Detail', 'Resolution Detail']:
    print(f"\nTopics in {column}:")

    # Splitting the text data into words
    text_data = [text.split() for text in ccoms[column]]

    # Perform LDA
    lda_model, dictionary = perform_lda(text_data)

    # Print the topics
    for idx, topic in lda_model.show_topics(formatted=False, num_words= 5):
        print("Topic: {} \nWords: {}".format(idx, [w for w, _ in topic]))

