# day-18
21 days sql challenge 
SELECT
    service_name,
    total_patients,
    total_patients - (SELECT AVG(total_patients) FROM
                        (SELECT service_name, COUNT(*) AS total_patients
                         FROM Admissions
                         GROUP BY service_name) AS t) AS difference_from_avg,
    CASE
        WHEN total_patients >
             (SELECT AVG(total_patients) FROM
                (SELECT service_name, COUNT(*) AS total_patients
                 FROM Admissions
                 GROUP BY service_name) AS t)
        THEN 'Above Average'

        WHEN total_patients =
             (SELECT AVG(total_patients) FROM
                (SELECT service_name, COUNT(*) AS total_patients
                 FROM Admissions
                 GROUP BY service_name) AS t)
        THEN 'Average'

        ELSE 'Below Average'
    END AS rank_indicator
FROM (
        -- Using UNION ALL to build service totals
        SELECT service_name, COUNT(*) AS total_patients
        FROM Admissions
        GROUP BY service_name
     ) AS sc
ORDER BY total_patients DESC;
