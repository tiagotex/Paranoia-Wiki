If you have an abstract base class that other classes inherit from, you cannot add `acts_as_paranoid` on the Abstract Class, otherwise you will get an empty table name in the query:

```ruby
class BaseClass < ActiveRecord::Base
  self.abstract_class = true
  
  acts_as_paranoid
end

class InheritedClass < BaseClass
end

>> InheritedClass.default_scoped.where_sql
# => "WHERE (``.deleted_at IS NULL)"
```

You have to put the `acts_as_paranoid` on the `InheritedClass` for it to use the correct table name in the scope.