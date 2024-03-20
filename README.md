proc sql;
    create table work.cumulative_results (Name char(50), Value char(50));
quit;

proc datasets lib=work nolist;
    delete cumulative_results; /* Deletes the dataset if it exists */
quit;
run;