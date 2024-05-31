# Value to find and tolerance
value_to_find = 0.112
tolerance = 0.0005

# Check if any value in 'Column2' is within the specified tolerance of the value_to_find
if np.isclose(df['Column2'], value_to_find, atol=tolerance).any():
    print(f'A value within {tolerance} of {value_to_find} exists in Column2.')
else:
    print(f'No value within {tolerance} of {value_to_find} exists in Column2.')
