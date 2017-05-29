# General

## Working with Stored Procedures

---

This section illustrates how to work with stored procedures in MySQL and Laravel.

Stored procedures are handy for encapsulating database intensive operations. Consider Stored Procedures as an API for your database.

### Reading From a Stored Procedure

```
DROP PROCEDURE IF EXISTS `select_by_user_id`;
delimiter ;;
CREATE PROCEDURE `select_by_user_id` (IN idx int)
BEGIN
	SELECT * FROM users WHERE id = idx;
END
 ;;
delimiter ;
```

Laravel Code:

```
$model = new App\User();
$user = $model->hydrate(
	DB::select(
		'call select_by_user_id(1)'
	)
);
```

By Hydrating the raw query to the model, we can return an [Eloquent Model](https://laravel.com/docs/master/eloquent).

If you need to load a stored procedure that does not require hydration to a model, you can simply call the raw query and return an array of your results.

```
$model = new App\User();
$user = DB::select(
	'call select_by_user_id(1)'
);
```

### Writing to a Stored Procedure

```
DROP PROCEDURE IF EXISTS `insert_user`;
delimiter ;;
CREATE PROCEDURE `insert_user` (IN uName varchar, IN uEmail varchar, IN uPassword varchar)
BEGIN
	INSERT INTO users (name, email, password) VALUES (uName, uEmail, uPassword);
END
 ;;
delimiter ;
```

Laravel Code:

```
DB:raw(
	'call insert_user(?, ?, ?),
	[
		$request->input('name'),
		$request->input('email'),
		Hash::make($request->input('password')),
	]
);
```

This Laravel code assumes you have done your validation via [form request validation](https://laravel.com/docs/master/validation#form-request-validation). (Never trust user input, always validate)

In the stored procedure you will notice that we type cast our inputs, this adds to extra security to ensure your data is correct.


### Notes:

- `DB:raw` creates a new raw query expression, but does not return data. Use `DB:select` to return data.
- With proper planning and structure you can create a CRUD-like API for your app.
- Laravel code may not have to change when making schema changes or performance updated to the database.
- You can set permissions on your database so `DELETE` can only be run from a stored procedure.


## Updating config at run time

---

While most config settings are static there are times we may want to update the config parameters at run time. To do this we set configuration variables by passing an array of key / value pairs.

### Set config

```
config(['app.debug' => true]);
```
