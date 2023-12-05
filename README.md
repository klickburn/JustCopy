# Part 3: Overall Communication Patterns

# Most and Least Used Communication Methods
most_used_comm_method = communication_aggregated[['E-Letter', 'Email', 'Letter', 'Live Chat', 'Text']].mean().idxmax()
least_used_comm_method = communication_aggregated[['E-Letter', 'Email', 'Letter', 'Live Chat', 'Text']].mean().idxmin()

# Variability in Communication
communication_variability = communication_aggregated[['E-Letter', 'Email', 'Letter', 'Live Chat', 'Text']].std()

# Correlation with Call Activities
call_activities = ['dial_cnt', 'contact_cnt', 'promise_cnt']
correlation_with_calls = communication_aggregated[call_activities + ['E-Letter', 'Email', 'Letter', 'Live Chat', 'Text']].corr().loc[call_activities]

overall_communication_patterns = {
    "Most Used Communication Method": most_used_comm_method,
    "Least Used Communication Method": least_used_comm_method,
    "Variability in Communication": communication_variability.to_dict(),
    "Correlation with Call Activities": correlation_with_calls.to_dict()
}

overall_communication_patterns
