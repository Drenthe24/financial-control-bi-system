# DAX Measures

This document contains a selection of DAX measures used to calculate financial KPIs in the Power BI financial control model.

Sensitive table names, account codes and company-specific business logic have been anonymized.

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
