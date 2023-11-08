import os
import re
import pandas as pd
import hashlib

# Add your list of specific tables here (as lowercase to match the extraction)
specific_tables = {'table1', 'table2', 'table3'}  # Replace with your actual table names

def file_hash(file_path):
    try:
        hasher = hashlib.sha256()
        with open(file_path, 'rb') as f:
            buf = f.read(65536)
            while len(buf) > 0:
                hasher.update(buf)
                buf = f.read(65536)
        return hasher.hexdigest()
    except PermissionError:
        print(f"Permission denied: {file_path}")
        return None

def find_sas_files(directories):
    sas_files = []
    processed_files = set()
    for directory in directories:
        for root, _, files in os.walk(directory):
            for file in files:
                if file.endswith('.sas'):
                    file_path = os.path.join(root, file)
                    hash_val = file_hash(file_path)
                    if hash_val is not None and hash_val not in processed_files:
                        processed_files.add(hash_val)
                        sas_files.append(file_path)
    return sas_files

def extract_sql_queries(sas_code):
    queries = re.findall(r'proc sql;\s*connect to.*?select.*?from.*?quit;', sas_code, re.DOTALL | re.IGNORECASE)
    return queries

def extract_schema_tables(sql_queries, specific_tables):
    all_tables = set()
    for query in sql_queries:
        matches = re.findall(r'\bfrom\s+(\w+)\.(\w+)|\bjoin\s+(\w+)\.(\w+)', query, re.IGNORECASE)
        for match in matches:
            table = (match[1] or match[3]).lower()
            all_tables.add(table)
    # Return True if all tables in the file are in the specific tables list
    return all_tables.issubset(specific_tables)

def export_to_csv(data, output_file):
    df = pd.DataFrame(list(data), columns=['SAS_File'])
    df.to_csv(output_file, index=False)

if __name__ == "__main__":
    directories = ['/svcbpa/PRD/', '/another/directory/']
    sas_files = find_sas_files(directories)
    
    matching_sas_files = set()
    for sas_file in sas_files:
        try:
            with open(sas_file, 'r') as file:
                sas_code = file.read()
            sql_queries = extract_sql_queries(sas_code)
            if extract_schema_tables(sql_queries, specific_tables):
                matching_sas_files.add(sas_file)
        except PermissionError:
            print(f"Permission denied: {sas_file}")

    print(f"Total number of matching SAS files: {len(matching_sas_files)}")
    print("SAS files exclusively using the specified tables:")
    for file in matching_sas_files:
        print(file)

    export_to_csv(matching_sas_files, 'exclusive_matching_sas_files.csv')
    print("Results exported to 'exclusive_matching_sas_files.csv'")
