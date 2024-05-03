# Format the datetime column into different formats
df['Date_only'] = df['Date'].dt.strftime('%Y-%m-%d')
df['Time_only'] = df['Date'].dt.strftime('%H:%M:%S')
df['Month_Day_Year'] = df['Date'].dt.strftime('%m/%d/%Y')
df['Full_DateTime'] = df['Date'].dt.strftime('%B %d, %Y %H:%M:%S')

print(df)
