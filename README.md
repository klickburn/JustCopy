import os
import re

def find_sas_files(directory):
    sas_files = []
    for root, _, files in os.walk(directory):
        for file in files:
            if file.endswith('.sas'):
                sas_files.append(os.path.join(root, file))
    return sas_files

def extract_sql_queries(sas_code):
    # Adjusted pattern to find SQL queries within SAS code
    # The pattern now looks for "proc sql;" followed by any characters (non-greedy),
    # then a newline and "connect to" (case-insensitive).
    queries = re.findall(r'proc sql;.*?\n\s*connect to.*?quit;', sas_code, re.DOTALL | re.IGNORECASE)
    return queries

def extract_schema_tables(sql_queries):
    schema_tables = set()
    for query in sql_queries:
        matches = re.findall(r'(\w+)\.(\w+)', query)
        schema_tables.update(matches)
    return schema_tables

if __name__ == "__main__":
    directory = '/svcbpa/PRD/'
    sas_files = find_sas_files(directory)
    
    all_schema_tables = set()
    for sas_file in sas_files:
        with open(sas_file, 'r') as file:
            sas_code = file.read()
        sql_queries = extract_sql_queries(sas_code)
        schema_tables = extract_schema_tables(sql_queries)
        all_schema_tables.update(schema_tables)

    print("All schema and tables found:")
    for schema, table in all_schema_tables:
        print(f"{schema}.{table}")