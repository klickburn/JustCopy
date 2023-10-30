
  def export_to_excel(data, output_file):
    # Convert set of tuples to list of dictionaries
    data_dicts = [{'Schema': schema, 'Table': table} for schema, table in data]
    # Create DataFrame
    df = pd.DataFrame(data_dicts)
    # Remove duplicates, sort, and reset index
    df = df.drop_duplicates().sort_values(by=['Schema', 'Table']).reset_index(drop=True)
    # Write to Excel
    df.to_excel(output_file, index=False)