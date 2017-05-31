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
