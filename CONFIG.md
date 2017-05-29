# Config

## prepend()

Prepend a value onto an array configuration value.

| Type | Variable | Status|  
|---|---|---|
| string  | $key | Required |
| mixed | $value | Required |

## Example

```
Config::prepend(
	'services.some_api',
	[
		'key' => env('SOME_API_KEY'),
		'secret' => env('SOME_API_SECRET'),
	]
);
```
