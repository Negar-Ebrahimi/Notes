# PostgreSQL


## [Transactions](https://www.postgresql.org/docs/8.3/tutorial-transactions.html)

- A transaction is said to be atomic: from the point of view of other transactions, it either happens completely or not at all. When multiple transactions are running concurrently, each one should not be able to see the incomplete changes made by others.

-  A transactional database guarantees that all the updates made by a transaction are logged in permanent storage (i.e., on disk) before the transaction is reported complete.

- PostgreSQL actually treats every SQL statement as being executed within a transaction. If you do not issue a BEGIN command, then each individual statement has an implicit BEGIN and (if successful) COMMIT wrapped around it. A group of statements surrounded by BEGIN and COMMIT is sometimes called a transaction block.

- Savepoints allow you to selectively discard parts of the transaction, while committing the rest. After defining a savepoint with SAVEPOINT, you can if needed roll back to the savepoint with ROLLBACK TO. All the transaction's database changes between defining the savepoint and rolling back to it are discarded, but changes earlier than the savepoint are kept.

## [Inheritance](https://www.postgresql.org/docs/8.3/tutorial-inheritance.html)
```
CREATE TABLE cities (
    name            text,
    population      float,
    altitude        int     -- in feet
);

CREATE TABLE capitals (
    state           char(2)
) INHERITS (cities);
```
```
SELECT name, altitude
    FROM ONLY cities
    WHERE altitude > 500;
```
```
SELECT name, altitude
    FROM cities*
    WHERE altitude > 500;
```

## [Data Types](https://www.postgresql.org/docs/8.3/datatype.html)
### Enumerated Types
```CREATE TYPE mood AS ENUM ('sad', 'ok', 'happy');
CREATE TABLE person (
    name text,
    current_mood mood
)
```
