# Medical Claims and Patient Analytics Project

## Project purpose

This portfolio project analyzes synthetic healthcare claims, patient history, and provider activity. It is built for a Power BI dashboard that tracks claim handling, total claim amount, claims per patient, provider performance, and patient geography.

## Cleaned model files

- `DimPatient.csv` — de-identified patient demographics, location, age, income, and healthcare totals
- `DimProvider.csv` — provider, specialty, organization, and provider location
- `DimPayer.csv` — payer reference data
- `DimDate.csv` — date dimension covering the claim service date range
- `FactClaims.csv` — one row per claim with financial metrics and handling status
- `FactEncounters.csv` — patient encounter history
- `FactConditions.csv` — patient condition history
- `data_quality_summary.csv` — row counts, duplicate checks, and missing-value counts

## Cleaning and business rules

- All date fields were parsed to date values.
- Claim transaction amounts were converted to numeric values and rolled up to the claim level.
- `Claim Amount` is the sum of transaction `AMOUNT` values where the transaction type is `CHARGE`.
- `Payment Amount` is the sum of transaction `PAYMENTS` values where the transaction type is `PAYMENT`.
- A claim is considered handled when its primary claim status is `Closed`.
- Sensitive identifiers and direct-contact fields were excluded from the patient dimension. The synthetic source data is nevertheless used only for demonstration.
- `FactClaims` is the financial source of truth for dashboard claim measures. Avoid summing total claim cost from encounters alongside it because that would double count.

## Power BI model



| From | To | Relationship |
|---|---|---|
| DimPatient[Patient_ID] | FactClaims[Patient_ID] | 1 to many |
| DimPatient[Patient_ID] | FactEncounters[Patient_ID] | 1 to many |
| DimPatient[Patient_ID] | FactConditions[Patient_ID] | 1 to many |
| DimProvider[Provider_ID] | FactClaims[Provider_ID] | 1 to many |
| DimProvider[Provider_ID] | FactEncounters[Provider_ID] | 1 to many |
| DimPayer[Payer_ID] | FactEncounters[Payer_ID] | 1 to many |
| DimDate[Date] | FactClaims[Service_Date] | 1 to many |

Mark `DimDate` as the date table using `Date`.





