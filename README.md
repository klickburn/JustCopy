# Check if the value exists in 'Column2'
if df['Column2'].isin([value_to_find]).any():
    print(f'The value {value_to_find} exists in Column2.')
else:
    print(f'The value {value_to_find} does not exist in Column2.')
