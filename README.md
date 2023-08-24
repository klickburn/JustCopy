def predict_next_touchpoints(current_channel, transition_matrix, num_predictions=3):
    """
    Predict the next touchpoints given a current channel and a transition matrix.
    """
    # Get the transition probabilities for the current channel
    next_probabilities = transition_matrix.loc[current_channel]
    
    # Exclude the PYMT channel as we want to predict touchpoints leading to payment
    next_probabilities = next_probabilities.drop('PYMT', errors='ignore')
    
    # Get the top channels with the highest probabilities
    top_channels = next_probabilities.sort_values(ascending=False).head(num_predictions).index.tolist()
    
    return top_channels

# Example: Predict the next 3 touchpoints for a customer whose last interaction was 'EMAIL'
sample_channel = 'EMAIL'
predicted_touchpoints = predict_next_touchpoints(sample_channel, transition_matrix)

predicted_touchpoints
