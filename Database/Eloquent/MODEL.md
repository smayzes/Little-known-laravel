# Model

Out of the box Laravel's Eloquent models are opinioated about the structure of your database table. Luckily you can make changes on your model files that help change these opinions to fit your expected data structure. 

## Changing the name of the created_at and updated_at columns

This use case might be valid when working with a legacy database and these fields are named differently from what Laravel expects (created_at, updated_at). In your model class, you can simply change these values to reflect the naming convention of your data structure. 

In this example, let's consider our pizza table tracks the time that the pizza was "cooked_at" and "reheated_at". 
```
class Pizza 
{
    /**
     * The name of the "created at" column.
     *
     * @var string
     */
    const CREATED_AT = 'coooked_at';
    
    /**
     * The name of the "updated at" column.
     *
     * @var string
     */
    const CREATED_AT = 'reheated_at';
}
```

Now, through using Eloquent these columns will be populated on Insert and Update. 

## Check if a Mutator or Accessor has been set

In an Eloquent Model you can set (mutators & accessors)[https://laravel.com/docs/5.4/eloquent-mutators#accessors-and-mutators] on your Model properties. There may be times where you might need to check if a mutator or accessor exists for this property. 

For example we have an accessor on the `cheese` property of our Pizza Model, this will return a boolean result:
```
// Retrieve a model by its primary key...
$pizza = App\Pizza::find(1);

// Returns boolean result based on accessor existence
echo $pizza-hasSetMutator('cheese'); 
```
We can also have a mutator on the same `cheese` property that we want to check against:

```
// Retrieve a model by its primary key...
$pizza = App\Pizza::find(1);

// Returns boolean result based on accessor existence
echo $pizza-hasGetMutator('cheese'); 
```

In the end if we ever need to get the original data that has not been mutated, we can run the following code: 
```
$pizza->getOriginal('name');
```

## Using Mutators & Accessors to create new Model properties

_@TODO Expand and provide better examples_

In my experience when working with a legacy database, the database structure isn't always ideal for my ideal output. Mutators and accessors help keep the logic out of my controller to make clean and consistent data for my storage medium. 

```
public function getSpecialSauceAttribute($value)
{
    return $this->regular_sauce . ', ' . $this->garlic_amount;
}
```

I can also use the mutator to set Model properties for the storage medium of choice. 

```
public function setSpecialSauceAttribute($value)
{
    list($this->regular_sauce, $thi->garlic_amount) = implode(', ', $this->special_sauce);
}
```

## Return a timestamp as DateTime object with time set to 00:00:00.
@TODO expand
```
// Retrieve a model by its primary key...
$pizza = App\Pizza::find(1);

echo $pizza-asDate('cooked_at'); 
```

