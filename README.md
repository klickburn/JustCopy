
# Feature engineering for logistic regression
data_dummies = pd.get_dummies(modified_events_df['channel'], drop_first=True)
data_dummies['date_ordinal'] = modified_events_df['date'].apply(lambda x: x.toordinal())
data_dummies['target'] = modified_events_df['channel'].apply(lambda x: 1 if x == 'PYMT' else 0)

# Split data into train and test sets and scale features
X = data_dummies.drop('target', axis=1)
y = data_dummies['target']

scaler = StandardScaler()
X_scaled = scaler.fit_transform(X)

X_train, X_test, y_train, y_test = train_test_split(X_scaled, y, test_size=0.2, random_state=42)

# Train and evaluate logistic regression model
logreg = LogisticRegression(max_iter=1000)
logreg.fit(X_train, y_train)

y_pred = logreg.predict(X_test)
accuracy = accuracy_score(y_test, y_pred)
classification_rep = classification_report(y_test, y_pred)

print(accuracy)
print(classification_rep)
This code provides an end-to-end solution, starting from creating the sample dataset to performing logistic regression and evaluating the results.





