# Project Title

Loan-Analysis-and-Financial-Risk-Dashboard

This dashboard helps financial institutions analyze loan data and assess risk more effectively.
It provides insights into borrower profiles, loan performance, and default patterns, enabling better decision-making and credit policy improvements.

By analyzing more than 255K loan records, the dashboard identifies the impact of factors such as income, employment type, credit score, loan purpose, and demographics on loan defaults.
Through trend and comparative analysis, it helps track Year-over-Year (YOY) and Year-to-Date (YTD) changes in loan performance.

This report is designed to support risk evaluation, creditworthiness analysis, and lending strategy optimization.

## Steps Followed

Step 1: Loaded the dataset (255,347 records) into Power BI Desktop for analysis.

Step 2: Used Power Query Editor for data cleaning, type transformation, and handling null values.

Step 3: Created calculated columns for:

- Year (from loan date)

- Age Group (Teen, Adult, Middle-Age Adult, Senior Citizen)

- Credit Score Bins (Very Low, Low, Medium, High)

- Income Bracket (Low, Medium, High)

Step 4: Built multiple DAX measures across three measure tables:

- Average Income by Employment Type

- Default Rate by Year and Employment Type

- YOY Loan and Default Loan Change

- YTD Loan Amount

Step 5: Designed visuals for loan distribution, borrower demographics, and risk trends using:

- Clustered bar/column charts

- Cards for KPIs (Average Loan, Default %, Total Loan)

- Line charts for YOY trends

- Pie charts for Age, Income, and Education segmentation

Step 6: Used slicers and filters for parameters like Year, Employment Type, Credit Bin, and Loan Purpose.

Step 7: Created three main report pages for:

- Loan and Default Overview

- Applicant Demographics & Financial Profile

- Financial Risk Trends (YOY, YTD Analysis)

## DAX Calculations 

 - "Loan and Default Overview"

1.Average Income by Employment Type = 
CALCULATE(AVERAGE(Loan_default[Income]), ALLEXCEPT('Loan_default', 'Loan_default'[EmploymentType]))

2.Default Rate by Year =
VAR TotalLoans = CALCULATE(COUNTROWS('Loan_default'), ALLEXCEPT('Loan_default', 'Loan_default'[Year]))
VAR Defaults = CALCULATE(COUNTROWS(FILTER('Loan_default', 'Loan_default'[Default] = TRUE())), ALLEXCEPT('Loan_default', 'Loan_default'[Year]))
RETURN DIVIDE(Defaults, TotalLoans) * 100

3.YOY Loan Amount Change =
DIVIDE(
    CALCULATE(SUM('Loan_default'[LoanAmount]), 'Loan_default'[Year] = YEAR(MAX('Loan_default'[Loan Date])) ) -
    CALCULATE(SUM('Loan_default'[LoanAmount]), 'Loan_default'[Year] = YEAR(MAX('Loan_default'[Loan Date])) - 1 ),
    CALCULATE(SUM('Loan_default'[LoanAmount]), 'Loan_default'[Year] = YEAR(MAX('Loan_default'[Loan Date])) - 1 ),
0) * 100


4.Default Rate by Year =
VAR totalloans =
    CALCULATE(
        COUNTROWS('Loan_default'),
        ALLEXCEPT('Loan_default', 'Loan_default'[Year])
    )
VAR default =
    CALCULATE(
        COUNTROWS(
            FILTER('Loan_default', 'Loan_default'[Default] = TRUE())
        ),
        ALLEXCEPT('Loan_default', 'Loan_default'[Year])
    )
RETURN
DIVIDE(default, totalloans) * 100

5.Loan Amount by Purpose =
SUMX(
    FILTER('Loan_default', NOT(ISBLANK('Loan_default'[LoanAmount]))),
    'Loan_default'[LoanAmount]
)



- "Applicant Demographics & Financial Profile"

1.Average Loan Amount (High Credit) =
AVERAGEX(
    FILTER('Loan_default', 'Loan_default'[Credit Score Bins] = "High"),
    Loan_default[LoanAmount]
)

2.Loan by Education Type =
COUNTROWS(
    FILTER('Loan_default', NOT(ISBLANK('Loan_default'[LoanID])))
)

3.Median by Credit Score Bins =
MEDIANX('Loan_default', 'Loan_default'[LoanAmount])

4.Total Loan (Credit Bins) =
CALCULATE(
    SUM('Loan_default'[LoanAmount]),
    'Loan_default'[Age Group] = "Adults",
    ALLEXCEPT(
        'Loan_default',
        Loan_default[Age],
        'Loan_default'[Age Group],
        'Loan_default'[CreditScore],
        'Loan_default'[Credit Score Bins]
    )
)

5.Total Loan (Middle Age Adults) =
SUMX(
    FILTER('Loan_default', 'Loan_default'[Age Group] = "Middle Age Adults"),
    'Loan_default'[LoanAmount]
)


- "Financial Risk Trends (YOY, YTD Analysis)"

1.YOY Default Loan Change =
DIVIDE(
    CALCULATE(
        COUNTROWS(
            FILTER('Loan_default', 'Loan_default'[Default] = TRUE())
        ),
        'Loan_default'[Year] = YEAR(MAX('Loan_default'[Loan Date (MM/DD/YYYY)]))
    ) -
    CALCULATE(
        COUNTROWS(
            FILTER('Loan_default', 'Loan_default'[Default] = TRUE())
        ),
        'Loan_default'[Year] = YEAR(MAX('Loan_default'[Loan Date (MM/DD/YYYY)])) - 1
    ),
    CALCULATE(
        COUNTROWS(
            FILTER('Loan_default', 'Loan_default'[Default] = TRUE())
        ),
        'Loan_default'[Year] = YEAR(MAX('Loan_default'[Loan Date (MM/DD/YYYY)])) - 1
    ),
0) * 100

2.YOY Loan Amount Change =
DIVIDE(
    CALCULATE(
        SUM('Loan_default'[LoanAmount]),
        'Loan_default'[Year] = YEAR(MAX('Loan_default'[Loan Date (MM/DD/YYYY)]))
    ) -
    CALCULATE(
        SUM('Loan_default'[LoanAmount]),
        'Loan_default'[Year] = YEAR(MAX('Loan_default'[Loan Date (MM/DD/YYYY)])) - 1
    ),
    CALCULATE(
        SUM('Loan_default'[LoanAmount]),
        'Loan_default'[Year] = YEAR(MAX('Loan_default'[Loan Date (MM/DD/YYYY)])) - 1
    ),
0) * 100

3.YTD Loan Amount =
CALCULATE(
    SUM('Loan_default'[LoanAmount]),
    DATESYTD('Loan_default'[Loan Date (MM/DD/YYYY)].[Date]),
    ALLEXCEPT('Loan_default', 'Loan_default'[Credit Score Bins], 'Loan_default'[MaritalStatus])
)


<p align="center">
  <img src="Images/img2.png" alt="Loan Default & Overview" width="800"/><br/>
  <span style="font-size:20px; font-weight:bold;">Loan Default & Overview</span>
</p>

