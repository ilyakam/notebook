# Redshift

AWS Redshift can be accessed directly from the Terminal with `psql`

## Examples

### `UNLOAD`

The [`UNLOAD` command](https://docs.aws.amazon.com/redshift/latest/dg/r_UNLOAD.html) is used to save database dumps onto AWS S3. The basic structure looks like this:

```sql
UNLOAD('SELECT * FROM «table»')
TO 's3://«location»'
CREDENTIALS 'aws_access_key_id=«key»;aws_secret_access_key=«secret»';
```

Hence, a common `UNLOAD` command may look like this in its near-final form:

```sql
UNLOAD('SELECT * FROM «table»')
TO 's3://«bucket»/«path».csv'
CREDENTIALS 'aws_access_key_id=«key»;aws_secret_access_key=«secret»'
ESCAPE
ADDQUOTES
PARALLEL OFF
DELIMITER AS ',';
```

#### Edge-Cases

* Instead of using double-quotes, use escaped single quotes, like so:
  ```sql
  UNLOAD('SELECT * FROM «table» WHERE «field»=\'«value»\'') TO 's3://«location»';
  ```

* It is impossible to `UNLOAD` with a `LIMIT` in the query. Therefore, [one solution](https://stackoverflow.com/a/32149417/545590) is to use a nested limit, like so:

  ```sql
  UNLOAD('SELECT * FROM «table» WHERE «id» IN (
    SELECT id FROM «table» LIMIT «limit»
  )') TO 's3://«location»';
  ```
