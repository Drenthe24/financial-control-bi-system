# DAX Measures

This document contains a selection of DAX measures used to calculate financial KPIs in the Power BI financial control model.

Sensitive table names, account codes and company-specific business logic have been anonymized.

## EBITDA

This measure calculates EBITDA by aggregating operating revenues and subtracting the main operating cost categories.

```DAX
EBITDA =
VAR TotalRevenues =
    ABS(
        [Operating Revenues]
        + [Inventory Change]
        + [Other Revenues]
    )

VAR TotalOperatingCosts =
    [Raw Material Costs]
    + [Service Costs]
    + [Lease and Rental Costs]
    + [Personnel Costs]
    + [Other Operating Expenses]

RETURN
    TotalRevenues - TotalOperatingCosts
```
Note: ABS() is used because revenues are stored with negative accounting sign in the source system.

## Net Financial Position (PFN / NFP)

This measure calculates the company's net financial position by aggregating financial debt categories and subtracting cash availability.

```DAX
Net Financial Position =
VAR BankDebtShortTerm =
    CALCULATE(
        SUM(AccountingEntries[Amount]),
        ChartOfAccounts[SecondLayerCode] = "BANK_DEBT_SHORT_TERM"
    )

VAR BankDebtLongTerm =
    CALCULATE(
        SUM(AccountingEntries[Amount]),
        ChartOfAccounts[SecondLayerCode] = "BANK_DEBT_LONG_TERM"
    )

VAR FinancialDebtShortTerm =
    CALCULATE(
        SUM(AccountingEntries[Amount]),
        ChartOfAccounts[SecondLayerCode] = "FINANCIAL_DEBT_SHORT_TERM"
    )

VAR FinancialDebtLongTerm =
    CALCULATE(
        SUM(AccountingEntries[Amount]),
        ChartOfAccounts[SecondLayerCode] = "FINANCIAL_DEBT_LONG_TERM"
    )

VAR Bonds =
    CALCULATE(
        SUM(AccountingEntries[Amount]),
        ChartOfAccounts[SecondLayerCode] = "BONDS"
    )

VAR TotalFinancialDebt =
    BankDebtShortTerm
    + BankDebtLongTerm
    + FinancialDebtShortTerm
    + FinancialDebtLongTerm
    + Bonds

RETURN
    -TotalFinancialDebt - [Cash Flow]
```
