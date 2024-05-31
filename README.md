substring_to_find = '0.112'

# Check each value in 'Column2' if the substring is in its string representation
matches = df[df['Column2'].apply(lambda x: substring_to_find in str(x))]

# Print the original values where matches were found
if not matches.empty:
    print('Matching values in Column2:')
    for value in matches['Column2']:
        print(value)
else:
    print(f'No values containing the substring "{substring_to_find}" found in Column2.')
