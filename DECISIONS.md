# Decisions

1. **Indexes.** `employees.emp_code` is unique for identity and joins; `(department, joined_on)` and `joined_on` serve headcount/trend filters; `(department, emp_code)` supports sorted employee pages. Unique `(emp_code, date)` enforces one attendance record per day and serves employee-month reads. `(date descending, emp_code ascending)` supports attendance listing, and `(date, late_minutes, emp_code)` supports monthly late aggregation. I considered a standalone status index, but date-first queries are better served by the compound indexes.

2. **Punch-in race.** Both requests may read the employee, but only one insert can satisfy the unique `(emp_code, date)` index. MongoDB rejects the other insert with `DuplicateKeyError`; the API maps that to 409. No check-then-insert is used as the concurrency guarantee.

3. **Ties.** The leaderboard uses MongoDB competition ranking over total late minutes. Everyone tied at the cutoff has the same rank and is returned if that rank is within `limit`; the next rank is skipped. Rows with equal totals are ordered by employee code.

4. **Headcount.** Department summary starts from eligible employees, not attendance logs, then looks up that month's logs. Preserving employees with no matching log means they still contribute one to headcount.

5. **100x data.** I would measure the actual query plans, then consider pre-aggregated daily/monthly summaries and a sharding key aligned with employee/date access. I would keep correctness checks against raw attendance data before switching reads to summaries.
