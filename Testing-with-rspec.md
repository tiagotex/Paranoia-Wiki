There is also an [rspec matcher](https://github.com/radar/paranoia/blob/rails4/lib/paranoia/rspec.rb) available with the gem


You can use shared examples to help test that models are including `acts_as_paranoid` properly. For example, the following shared examples can be created in `spec/support/shared_examples/paranoia_examples.rb`:

```ruby
shared_examples_for 'a Paranoid model' do

  it { should have_db_column(:deleted_at) } 

  it { should have_db_index(:deleted_at) }

  it 'adds a deleted_at where clause' do
    expect(described_class.default_scoped.where_sql).to include("`deleted_at` IS NULL")
  end

  it 'skips adding the deleted_at where clause when unscoped' do
    expect(described_class.unscoped.where_sql.to_s).not_to include("deleted_at")  # to_s to handle nil.
  end

end
```

And then all you need to do is include this in the specs for the model that should be acting as paranoid, like so:

```ruby
it_behaves_like 'a Paranoid model'
```