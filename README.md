import os
import re
import pandas as pd

def find_sas_files(directories):
    sas_files = []
    for directory in directories:
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
                schema_tables.add((match[0].lower(), match[1].lower()))
            elif match[2]:
                schema_tables.add((match[2].lower(), match[3].lower()))
    return schema_tables

def export_to_csv(data, output_file):
    data_dicts = [{'Schema': schema, 'Table': table, 'Occurrences': occurrences} for (schema, table), occurrences in data.items()]
    df = pd.DataFrame(data_dicts)
    df = df.groupby(['Schema', 'Table'])['Occurrences'].sum().reset_index()
    df = df.sort_values(by=['Schema', 'Table']).reset_index(drop=True)
    df.to_csv(output_file, index=False)

if __name__ == "__main__":
    directories = ['/svcbpa/PRD/', '/another/directory/']
    sas_files = find_sas_files(directories)
    
    all_schema_tables = {}
    for sas_file in sas_files:
        with open(sas_file, 'r') as file:
            sas_code = file.read()
        sql_queries = extract_sql_queries(sas_code)
        schema_tables = extract_schema_tables(sql_queries)
        for schema_table in schema_tables:
            all_schema_tables[schema_table] = all_schema_tables.get(schema_table, 0) + 1

    print(f"Total number of SAS files traversed: {len(sas_files)}")
    print("All schema and tables found with their occurrences:")
    for (schema, table), occurrences in all_schema_tables.items():
        print(f"{schema}.{table}: {occurrences} occurrences")

    export_to_csv(all_schema_tables, 'output.csv')
    print("Results exported to 'output.csv'")
