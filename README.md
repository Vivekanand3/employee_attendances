<<<<<<< HEAD
# Employee Attendance & Analytics API

FastAPI and MongoDB API for employee records, attendance punch-in/out, manual regularization, monthly and department
analytics, late-arrival ranking, daily department trends, and MongoDB query explains. The implementation is in
`app/main.py`; the request/response contract and business rules are in `openapi.yaml`.

## Run locally

Install the packages in `requirements.txt`, then set `MONGO_URI` and `MONGO_DB` as environment variables (or in a local,
untracked `.env` file). Use MongoDB 6.0 or newer. From the project root:

```powershell
python -m uvicorn app.main:app --reload
```

Interactive API documentation is available at `http://127.0.0.1:8000/docs`. The punch endpoints accept `punched_at` as
a 24-hour `HH:MM` time in IST; omitted times use the current time. Never commit `.env` or database credentials.
=======
# employee_attendance
>>>>>>> a1175a09c9acc25848952333c47c25e0edb19b2a
