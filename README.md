# pqsql-crash-fix Patch

This repository contains the `pqsql-crash-fix.diff` patch, which addresses a potential crash in the `pqsql` database handler of a Newserv-based server.

## Purpose

The patch prevents a null pointer dereference in the `dbhandler` function of `pqsql/pqsql.c`. This issue could occur if `queryhead` is `NULL` when database events are handled, leading to a server crash.

## Patch Details

The patch adds a safety check to ensure `queryhead` is not `NULL` before accessing its members or calling its handler function. The relevant code change is as follows:

```diff
+    // Prevent NULL pointer access
+    if (!queryhead)
+      return;
```

This early return ensures that no further operations are performed if `queryhead` is `NULL`, thus preventing a potential crash.

## Applying the Patch

To apply the patch:

1. Download the patch file:
   - [`pqsql-crash-fix.diff`](pqsql-crash-fix.diff)

2. In the root directory of your Newserv source tree, run:

   ```sh
   patch -p1 < /path/to/pqsql-crash-fix.diff
   ```

3. Rebuild your project as usual.

## File Reference

- **Target file:** `pqsql/pqsql.c`
- **Main function modified:** `dbhandler(int fd, short revents)`

## Author

[@WarPigs1602](https://github.com/WarPigs1602)

## License

Please refer to the license of the upstream Newserv repository and any additional notices in this repository.
