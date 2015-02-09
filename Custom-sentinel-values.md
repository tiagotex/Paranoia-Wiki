Most databases ignore null columns when it comes to resolving unique index
constraints.  This means unique constraints that involve nullable columns may be
problematic. Instead of using `NULL` to represent a not-deleted row, you can pick
a value that you want paranoia to mean not deleted. Note that you can/should
now apply a `NOT NULL` constraint to your `deleted_at` column.

Per model:

```ruby
# pick some value
acts_as_paranoid sentinel_value: DateTime.new(0)
```

or globally in a rails initializer, e.g. `config/initializer/paranoia.rb`

```ruby
Paranoia.default_sentinel_value = DateTime.new(0)
```