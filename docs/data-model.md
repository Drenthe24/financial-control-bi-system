# Data Model Design

## Overview

The financial control system is based on a structured data model designed to transform raw accounting data into decision-ready information.

The core idea is to separate:
- raw accounting data
- business logic
- financial aggregation rules

## Fact Table

The model is centered on a fact table:

- AccountingEntries

This table contains all accounting movements and represents the single source of truth for financial data.

## Chart of Accounts Structure

The chart of accounts has been reorganized into a multi-layer structure.

Each account is mapped into hierarchical levels (layers) to support:

- financial statement reclassification
- aggregation of costs and revenues
- flexible KPI calculation
- scalability of the model

## Multi-Layer Logic

Instead of calculating KPIs directly on raw accounts, the system uses intermediate aggregation layers.

Example:

- Level 1 → individual accounts
- Level 2 → grouped accounts (e.g. cost categories)
- Level 3 → financial statement structure

This approach allows:

- reuse of logic across multiple measures
- consistency in financial calculations
- easier maintenance and extension

## Why This Design

This model was designed to solve common problems:

- fragmented financial data
- hardcoded calculations
- lack of flexibility in reporting

By structuring the chart of accounts into layers, the system enables:

- dynamic KPI calculation
- simplified DAX measures
- scalable reporting architecture

## Example

Measures like EBITDA are not calculated directly from raw data, but by aggregating values from structured layers:

Operating Revenues → Layer aggregation  
Operating Costs → Layer aggregation  

This ensures consistency and reduces duplication of logic.
