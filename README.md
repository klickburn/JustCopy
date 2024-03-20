%macro processName(name);
    proc sql;
        create table work.result_&name. as
        select *
        from mydata
        where Name = "&name";
    quit;
%mend processName;