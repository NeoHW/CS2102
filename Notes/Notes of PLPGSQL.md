x## Notes of PL/PGSQL



### Meaning of Cursors:

1. **Why Cursors?**:

   - Most SQL operations (like `SELECT`, `INSERT`, `UPDATE`) work on sets of rows in a single operation. However, there are scenarios in application or database logic where you need to work on individual rows, one at a time. This is where cursors come in.
   - Cursors are especially useful in procedural database languages (like PL/SQL for Oracle or PL/pgSQL for PostgreSQL) where certain row-by-row operations might be required.

2. **Lifecycle of a Cursor**:

   - **Declaration**: Before you can use a cursor, you need to declare it. The declaration specifies an SQL statement associated with the cursor.
   - **Opening**: Once declared, a cursor needs to be opened. Opening the cursor executes the SQL statement associated with it and prepares the result set.
   - **Fetching**: After opening, you can retrieve rows from the cursor one at a time. This is typically done in a loop.
   - **Closing**: After you've finished processing all rows, you should close the cursor to release associated memory and resources.

3. **Example** (in the context of PL/pgSQL in PostgreSQL):

   ```sql
   DECLARE
       my_cursor CURSOR FOR SELECT column1, column2 FROM my_table WHERE some_condition;
       rec RECORD;
   BEGIN
       OPEN my_cursor;
       LOOP
           FETCH my_cursor INTO rec;
           rec.column1
           EXIT WHEN NOT FOUND;  -- Exit the loop if no more rows to fetch
           
           -- Process each row here using rec.column1, rec.column2, etc.
       END LOOP;
       
       CLOSE my_cursor;
   END;
   ```

4. **Limitations and Cautions**:

   - **Performance**: Cursors can be slower than set-based operations since they process rows one at a time.
   - **Resource Utilization**: Keeping a cursor open consumes resources on the database system, especially if the result set is large. It's important to close cursors once you're done with them.
   - **Use When Necessary**: It's generally recommended to use cursors only when necessary. If an operation can be achieved using set-based SQL statements, that's often preferable.



### Meaning of FOUND:

**With Cursors**: When fetching from a cursor using the `FETCH` command, `FOUND` can be used to determine if the fetch was successful. If the `FETCH` successfully retrieves a row, `FOUND` is set to `true`; if there are no more rows to retrieve, `FOUND` becomes `false`.

```SQL
FETCH curs INTO some_var;
IF FOUND THEN
    -- A row was successfully fetched from the cursor
ELSE
    -- No more rows left in the cursor
END IF;
```



### RETURN NEXT

In the context of PL/pgSQL (the procedural language used within PostgreSQL functions), `RETURN NEXT` is a command used within functions that return a set of rows, often defined by a composite type or a `TABLE` type.

Here's a breakdown:

1. **Purpose**: The primary purpose of `RETURN NEXT` is to add the current row to the result set of a function. This allows you to build up the result set row-by-row during the function's execution.

2. **Usage Context**: It's commonly used within looping constructs when you're iterating over some data and want to return multiple rows from the function.

3. **How it works**:
   - When `RETURN NEXT` is executed, the current values of the variables listed in the function's `OUT` parameters or the `TABLE` return type are added as a new row to the function's result set.
   - After adding the row, the function continues its execution, allowing for further rows to be added if `RETURN NEXT` is executed again.
   - The final result set of the function will consist of all rows added via `RETURN NEXT` during the function's execution.

4. **Example**:
   If you have a function defined to return a `TABLE` with columns `a` and `b`, you might use `RETURN NEXT` like this:

   ```sql
   CREATE OR REPLACE FUNCTION my_function() RETURNS TABLE(a int, b int) AS $$
   DECLARE
       -- some declarations
   BEGIN
       -- some processing
       a := 1; 
       b := 2;
       RETURN NEXT;  -- This adds a row to the result set with values (1, 2)
   
       a := 3;
       b := 4;
       RETURN NEXT;  -- This adds another row to the result set with values (3, 4)
       
       -- and so on...
       
       RETURN;
   END;
   $$ LANGUAGE plpgsql;
   ```

   When you call `my_function()`, it will return a result set with the rows `(1,2)` and `(3,4)`.

5. **Ending the Function**: While `RETURN NEXT` adds a row to the result set, the function execution continues. To finish the function and return all collected rows, you typically use a plain `RETURN;` statement, as shown in the example above.

In your previously provided function `list_r_avg`, the `RETURN NEXT;` is used to add each student's average score (after excluding the highest and lowest scores) to the result set of the function.