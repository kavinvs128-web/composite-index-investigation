## Incorrect Index Analysis

The index was created as:

CREATE INDEX idx_wrong ON employees(salary, department);

This index is ineffective because PostgreSQL follows the Left-Most Prefix Rule.

The query filters first on department, but the index starts with salary.

Since salary is a range condition (> 50000), PostgreSQL cannot efficiently use the second column (department) in the index.

As a result, the database performs a sequential scan instead of using the index.



## Correct Index Optimization

The corrected index:

CREATE INDEX idx_correct ON employees(department, salary);

This works because:

- department is an equality condition
- salary is a range condition

PostgreSQL can:
1. First filter by department
2. Then apply salary filter efficiently

This follows the Left-Most Prefix Rule, allowing full index utilization.

## Left-Most Prefix Rule

PostgreSQL uses a composite index from left to right.

For index (A, B):

Works for:
- WHERE A = ?
- WHERE A = ? AND B = ?

Does NOT work efficiently for:
- WHERE B = ?