# Postgres Commands
- Reset the sequence of an autonumber; [https://stackoverflow.com/questions/5342440/reset-auto-increment-counter-in-postgres](https://stackoverflow.com/questions/5342440/reset-auto-increment-counter-in-postgres)
    - If you created the table product with an id column, then the sequence is not simply called product, but rather product_id_seq (that is, ${table}_${column}_seq).
    - `ALTER SEQUENCE my_schema.product_id_seq RESTART WITH 1453`
