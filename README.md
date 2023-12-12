# Adding a total column to the dataframe to show the total number of accounts enrolled in each bucket

enrollments_per_bucket_program['Total'] = enrollments_per_bucket_program.sum(axis=1)
enrollments_per_bucket_program
