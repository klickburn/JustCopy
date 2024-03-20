%let names=John Jane Doe Smith; /* Define a list of names separated by spaces */
%let n=%sysfunc(countw(&names)); /* Count the number of names in the list */

%macro loopNames;
    %do i=1 %to &n;
        %let currentName=%scan(&names, &i); /* Extract the current name from the list */
        %processName(&currentName); /* Call the macro for the current name */
    %end;
%mend loopNames;

%loopNames; /* Execute the loop */