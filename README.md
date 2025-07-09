# week - 8 

# 📅 Date Dimension Population Script (`PopulateDateDimension`)

## 📌 Overview

This T-SQL script creates and populates a `Date_Dimension` table with rich date attributes for **one full year**, based on a given input date. The year is inferred from the provided date, and **all 365/366 dates of that year** are inserted using a **single `INSERT` statement**, as required.

The script includes:
- Table creation
- Stored procedure definition
- Procedure execution
- Data validation queries

---

## 🗃️ Table Structure: `Date_Dimension`

| Column Name         | Description                            |
|---------------------|----------------------------------------|
| `Date`              | Actual date (Primary Key)              |
| `SKDate`            | Surrogate Key in `YYYYMMDD` format     |
| `KeyDate`           | Date in `MM/DD/YYYY` format            |
| `CalendarDay`       | Day of the month (1–31)                |
| `CalendarMonth`     | Month number (1–12)                    |
| `CalendarQuarter`   | Quarter (1–4)                          |
| `CalendarYear`      | Year                                   |
| `DayName`           | Full name of the weekday               |
| `DayNameShort`      | Short name (e.g., Mon, Tue)            |
| `DayNumberOfWeek`   | Weekday number (1=Sunday)              |
| `DayNumberOfYear`   | Day of year (1–365/366)                |
| `DaySuffix`         | Ordinal suffix (e.g., 1st, 2nd)        |
| `FiscalWeek`        | Week number of the year (1–53)         |
| `FiscalPeriod`      | Same as Calendar Month                 |
| `FiscalQuarter`     | Same as Calendar Quarter               |
| `FiscalYear`        | Same as Calendar Year                  |
| `FiscalYearPeriod`  | Concatenation of FiscalYear + MM       |

> 💡 Fiscal columns assume calendar-based fiscal settings. Modify logic if your organization follows a different fiscal calendar (e.g., 4-4-5).

---

## ⚙️ How It Works

### 1. Create the table

The script first creates the `Date_Dimension` table with all necessary columns.

### 2. Define the Stored Procedure

The `PopulateDateDimension` procedure accepts a `@InputDate` parameter and calculates the start and end of the year based on that date. It uses a recursive `WITH` CTE + `sys.all_objects` to generate a range of dates in one query and inserts them in a single shot.

### 3. Execute the Procedure

You execute the stored procedure with any date from the year you want to generate (e.g., `'2020-07-14'`).

---

## 🚀 How to Use

### ▶️ Execute the script in order:

```sql
-- Step 1: Create the table
CREATE TABLE Date_Dimension (...);

-- Step 2: Create the procedure
CREATE PROCEDURE PopulateDateDimension
    @InputDate DATE
AS
BEGIN
    -- logic here
END;

-- Step 3: Execute the procedure
EXEC PopulateDateDimension '2020-07-14';

-- Step 4: Verify output
SELECT TOP 10 * FROM Date_Dimension ORDER BY [Date];
