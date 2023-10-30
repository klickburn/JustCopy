import os
import re
import pandas as pd

def find_sas_files(directory):
    sas_files = []
    for root, _, files in os.walk(directory):
        for file in files:
            if file.endswith('.sas'):
                sas_files.append(os.path.join(root, file))
    return sas_files

def extract_sql_queries(sas_code):
    queries = re.findall(r'proc sql;\s*connect to.*?select.*?from.*?quit;', sas_code, re.DOTALL | re.IGNORECASE)
    return queries

def extract_schema_tables(sql_queries):
    schema_tables = set()
    for query in sql_queries:
        matches = re.findall(r'\bfrom\s+(\w+)\.(\w+)|\bjoin\s+(\w+)\.(\w+)', query, re.IGNORECASE)
        for match in matches:
            if match[0]:
                schema_tables.add((match[0], match[1]))
            elif match[2]:
                schema_tables.add((match[2], match[3]))
    return schema_tables

def export_to_excel(data, output_file):
    df = pd.DataFrame(data, columns=['Schema', 'Table'])
    df = df.drop_duplicates().sort_values(by=['Schema', 'Table']).reset_index(drop=True)
    df.to_excel(output_file, index=False)

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

    export_to_excel(all_schema_tables, 'output.xlsx')
    print("Results exported to 'output.xlsx'")