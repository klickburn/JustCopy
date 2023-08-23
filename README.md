# Find the maximum path length
max_path_length = conversion_path_lengths.max()

# Filter the paths that have this maximum length
longest_paths = conversion_paths[conversion_path_lengths == max_path_length]

# Convert the sequences to a string format for presentation
longest_paths_str = longest_paths.apply(lambda x: ' -> '.join(x))

longest_paths_str
