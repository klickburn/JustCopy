data _null_;
    * Get the current date;
    current_date = date();
    * Extract the year and month from the current date;
    current_year = year(current_date);
    current_month = month(current_date);
    
    * Handling year and month adjustments for calculations that span multiple months;
    if current_month = 1 then do;
        previous_month = 12;
        previous_month_year = current_year - 1;
        two_months_ago = 11;
        two_months_ago_year = current_year - 1;
    end;
    else if current_month = 2 then do;
        previous_month = 1;
        previous_month_year = current_year;
        two_months_ago = 12;
        two_months_ago_year = current_year - 1;
    end;
    else do;
        previous_month = current_month - 1;
        previous_month_year = current_year;
        two_months_ago = current_month - 2;
        two_months_ago_year = current_year;
    end;

    * Determining the last date of the previous month;
    last_date_previous_month = intnx('month', mdy(previous_month, 1, previous_month_year), 0, 'end');
    
    * Date comparison to decide on value assignments;
    if current_date >= mdy(current_month, 5, current_year) and current_date <= mdy(current_month, 18, current_year) then do;
        * Assign values for dates between the 5th and 18th;
        yymmxx = cats(put(current_year,4. - 2000), put(current_month, z2.), '01');
        reportcycle = yymmxx;
        month = upcase(put(current_date, monname3.));
        pyymmxx = cats(put(previous_month_year, 4. - 2000), put(previous_month, z2.), '02');
        ppyymmxx = cats(put(previous_month_year, 4. - 2000), put(previous_month, z2.), '01');
        reportdate = cats(put(previous_month_year, 4.), put(previous_month, z2.), put(day(last_date_previous_month), z2.));
        last_yr_mo = cats(put(current_year, 4.), put(current_month, z2.));
        firstdate = '01-' || upcase(put(current_date, monname3.)) || '-' || put(current_year, 4.);
        lastdate = '15-' || upcase(put(current_date, monname3.)) || '-' || put(current_year, 4.);
        fdate9 = '01' || upcase(put(current_date, monname3.)) || put(current_year, 4.);
        ldate9 = '15' || upcase(put(current_date, monname3.)) || put(current_year, 4.);
        td_firstdate = cats(put(current_year, 4.), '-', put(current_month, z2.), '-', '01');
        td_lastdate = cats(put(current_year, 4.), '-', put(current_month, z2.), '-', '15');
        bdate9 = '01' || upcase(put(current_date, monname3.)) || put(current_year, 4.);
        mdate9 = '15' || upcase(put(current_date, monname3.)) || put(current_year, 4.);
        first_day = '01-' || upcase(put(current_date, monname3.)) || '-' || put(current_year, 4.);
        last_day = '15-' || upcase(put(current_date, monname3.)) || '-' || put(current_year, 4.);
        pr_first_day = '16-' || upcase(put(mdy(previous_month, 1, previous_month_year), monname3.)) || '-' || put(previous_month_year, 4.);
        pr_last_day = put(day(last_date_previous_month), z2.) || '-' || upcase(put(mdy(previous_month, 1, previous_month_year), monname3.)) || '-' || put(previous_month_year, 4.);
        pr_bdate9 = '16' || upcase(put(mdy(previous_month, 1, previous_month_year), monname3.)) || put(previous_month_year, 4.);
        pr_mdate9 = put(day(last_date_previous_month), z2.) || upcase(put(mdy(previous_month, 1, previous_month_year), monname3.)) || put(previous_month_year, 4.);
        * Continue from the previous point for the first scenario;
        pr_first_td = cats(put(previous_month_year, 4.), '-', put(previous_month, z2.), '-', '16');
        pr_last_td = cats(put(previous_month_year, 4.), '-', put(previous_month, z2.), '-', put(day(last_date_previous_month), z2.));
        auto_pr_mo = cats(put(previous_month_year, 4.), put(previous_month, z2.));
        td1_2mnthb4 = cats(put(two_months_ago_year, 4.), '-', put(two_months_ago, z2.), '-', '01');
        td1_1mnthb4 = cats(put(previous_month_year, 4.), '-', put(previous_month, z2.), '-', '01');
        td1_firstdate = cats(put(current_year, 4.), '-', put(current_month, z2.), '-', '01');
        * Assuming 31 as a generic last day for demonstration. Adjust based on actual month last day if necessary;
        td1_lastdate = cats(put(current_year, 4.), '-', put(current_month, z2.), '-', '31');
        last_mm = cats(put(previous_month_year, 4.), put(previous_month, z2.));
        yyyymm = cats(put(current_year, 4.), put(current_month, z2.));
    end;
    else do;
        * Assign values for dates between the 19th and 4th of the next month;
        * Since the logic might be similar but with different values, ensure to adjust variable assignments as per your second scenario;
        yymmxx = cats(put(current_year,4. - 2000), put(current_month, z2.), '02');
        reportcycle = yymmxx;
        month = upcase(put(current_date, monname3.));
        pyymmxx = cats(put(current_year, 4. - 2000), put(current_month, z2.), '01');
        ppyymmxx = cats(put(previous_month_year, 4. - 2000), put(previous_month, z2.), '02');
        reportdate = cats(put(current_year, 4.), put(current_month, z2.), '15');
        last_yr_mo = cats(put(previous_month_year, 4.), put(previous_month, z2.));
        firstdate = '01-' || upcase(put(current_date, monname3.)) || '-' || put(current_year, 4.);
        * Adjust based on actual month last day;
        lastdate = put(day(intnx('month',current_date,0,'end')), z2.) || '-' || upcase(put(current_date, monname3.)) || '-' || put(current_year, 4.);
        fdate9 = '01' || upcase(put(current_date, monname3.)) || put(current_year, 4.);
        ldate9 = put(day(intnx('month',current_date,0,'end')), z2.) || upcase(put(current_date, monname3.)) || put(current_year, 4.);
        td_firstdate = cats(put(current_year, 4.), '-', put(current_month, z2.), '-', '16');
        * Adjust based on actual month last day;
        td_lastdate = cats(put(current_year, 4.), '-', put(current_month, z2.), '-', put(day(intnx('month',current_date,0,'end')), z2.));
        bdate9 = '16' || upcase(put(current_date, monname3.)) || put(current_year, 4.);
        mdate9 = put(day(intnx('month',current_date,0,'end')), z2.) || upcase(put(current_date, monname3.)) || put(current_year, 4.);
        first_day = '16-' || upcase(put(current_date, monname3.)) || '-' || put(current_year, 4.);
        * Adjust based on actual month last day;
        last_day = put(day(intnx('month',current_date,0,'end')), z2.) || '-' || upcase(put(current_date, monname3.)) || '-' || put(current_year, 4.);
        pr_first_day = '01-' || upcase(put(current_date, monname3.)) || '-' || put(current_year, 4.);
        pr_last_day = '15-' || upcase(put(current_date, monname3.)) || '-' || put(current_year, 4.);
        pr_bdate9 = '01' || upcase(put(current_date, monname3.)) || put(current_year, 4.);
        pr_mdate9 = '15' || upcase(put(current_date, monname3.)) || put(current_year, 4.);
        pr_first_td = cats(put(current_year, 4.), '-', put(current_month, z2.), '-', '01');
        pr_last_td = cats(put(current_year, 4.), '-', put(current_month, z2.), '-', '15');
        auto_pr_mo = cats(put(previous_month_year, 4.), put(previous_month, z2.));
        td1_2mnthb4 = cats(put(two_months_ago_year, 4.), '-', put(two_months_ago, z2.), '-', '01');
        td1_1mnthb4 = cats(put(previous_month_year, 4.), '-', put(previous_month, z2.), '-', '01');
        td1_firstdate = cats(put(current_year, 4.), '-', put(current_month, z2.), '-', '01');
        td1_lastdate = cats(put(current_year, 4.), '-', put(current_month, z2.), '-', put(day(last_date_current_month), z2.));
        last_mm = cats(put(previous_month_year, 4.), put(previous_month, z2.));
        yyyymm = cats(put(current_year, 4.), put(current_month, z2.));
    end;
