# Calculating the average call activities and communication frequencies across all accounts

# Average Call Activities
average_dial_attempts = communication_aggregated['dial_cnt'].mean()
average_successful_contacts = communication_aggregated['contact_cnt'].mean()
average_promises_to_pay = communication_aggregated['promise_cnt'].mean()

# Average Communication Frequencies
average_eletter = communication_aggregated['E-Letter'].mean()
average_email = communication_aggregated['Email'].mean()
average_letter = communication_aggregated['Letter'].mean()
average_live_chat = communication_aggregated['Live Chat'].mean()
average_text = communication_aggregated['Text'].mean()

# Summary of the averages
average_summary = {
    "Average Dial Attempts": average_dial_attempts,
    "Average Successful Contacts": average_successful_contacts,
    "Average Promises to Pay": average_promises_to_pay,
    "Average E-Letter": average_eletter,
    "Average Email": average_email,
    "Average Letter": average_letter,
    "Average Live Chat": average_live_chat,
    "Average Text": average_text
}

average_summary
