# Python / Flask / FastAPI Specific Checks

Additional checks for Python web projects. Supplements the main scoring rubric.

---

## Security (Python)

- [ ] No hardcoded secrets (check for strings that look like keys/passwords)
- [ ] SQL queries use parameterized queries or ORM (SQLAlchemy, Django ORM)
- [ ] No \`eval()\` or \`exec()\` with user input
- [ ] No \`pickle.loads()\` on untrusted data
- [ ] Flask: \`SECRET_KEY\` not hardcoded
- [ ] FastAPI: dependency injection for auth (not manual checks)
- [ ] CORS configured with specific origins (not \`allow_origins=["*"]\`)
- [ ] File uploads validated (type, size, content)

## Error Handling (Python)

- [ ] Route handlers have try/except blocks
- [ ] Specific exceptions caught (not bare \`except:\`)
- [ ] Custom error handlers for 404, 500
- [ ] API errors return JSON with status codes (not HTML error pages)
- [ ] Database operations have error handling
- [ ] No \`print()\` for logging in production (use \`logging\` module)

## Code Structure (Python)

- [ ] Follows project structure conventions (blueprints for Flask, routers for FastAPI)
- [ ] No files over 500 lines
- [ ] Type hints on public functions
- [ ] Requirements pinned to specific versions (\`==\` not \`>=\`)
- [ ] Virtual environment used (venv, poetry, pipenv)
- [ ] \`__init__.py\` files where needed

## Performance (Python)

- [ ] Async endpoints for I/O operations (FastAPI)
- [ ] Database connection pooling configured
- [ ] No N+1 query patterns (eager loading where needed)
- [ ] Caching for expensive operations
- [ ] Background tasks for slow operations (Celery, RQ, or async)

## Deployment Readiness (Python)

- [ ] \`requirements.txt\` or \`pyproject.toml\` exists with all dependencies
- [ ] \`python -m py_compile\` passes on all files
- [ ] \`pip audit\` shows no critical vulnerabilities
- [ ] Gunicorn/uvicorn configured for production (not Flask dev server)
- [ ] Environment variables for all configuration
- [ ] .gitignore covers __pycache__, .env, venv/
