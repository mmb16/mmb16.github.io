---
layout: post
title:      "Data Integrity in Ruby on Rails"
date:       2018-02-09 21:09:07 -0500
permalink:  data_integrity_in_ruby_in_rails
---


Managing SQL migrations is a difficult part of learning to code. Once a migration is created and migrated to the database you should not delete it or delete the schema. Additionally, you should not change the schema. This is why migrations are particularly tricky–you need to determine the best schema before working with it in practice.

Lets go through some common commads and SQL code to use when building out a database without comprimising its integrity.

Creating a blank table called 'users' using ActiveRecord:

Command
```
rails generate migration CreateUsers
```

Output
```
class CreateUsers < ActiveRecord::Migration[5.0]
  def change
    create_table :users do |t|
    end
  end
end
```

From here we can directly type in the columns that we want. 
**Do NOT Migrate the table ```rake db:migrate``` until after you have typed the columns you want**

What if you already migrated the table without typing in the columns?

You just create a new migration to add the columns 

Command
```
rails generate migration AddEmailToUsers email:text
```

Result
```
class AddEmailToUsers < ActiveRecord::Migration[5.0]
  def change
    add_column :users, :email, :text
  end
end
```

Once you migrate this to the database using ```rake db:migrate``` your schema will look like this

```
ActiveRecord::Schema.define(version: 20180209175749) do

  create_table "users", force: :cascade do |t|
    t.text     "email"
    t.datetime "created_at", null: false
    t.datetime "updated_at", null: false
  end
	...
	```
	
	So the best way to track what the current state of your database looks like is by checking the schema, but do **NOT** edit, or delete it.
	
	Now let's say we want to rename to column we created.
	
	Command
	```
	rails generate migration FixColumnName
	```
	
	Output
	```
 class FixColumnName < ActiveRecord::Migration
  def change
  end
end
```

This gave us a blank migration, within the "change" method we can type in the action we want in this case its "rename_column" so our code should look like this:

```
class FixColumnName < ActiveRecord::Migration
  def change
    rename_column :users, :email, :username
  end
end
```

Then we run ```rake db:migrate``` in the terminal it will update our schema to look like this:

```
ActiveRecord::Schema.define(version: 20180209175749) do

  create_table "users", force: :cascade do |t|
    t.text     **"username"**
    t.datetime "created_at", null: false
    t.datetime "updated_at", null: false
  end
	...
	```

There are a bunch of change methods you can use within a migration, you can find them [here](http://edgeguides.rubyonrails.org/active_record_migrations.html#using-the-change-method)
	
The reason we use new migration instead of deleting an old table and generating a new one with is because that would comprise the data. With a new project with no data in it, there would not be an issue. However, if you were editing an existing project with hundreds or thousands of users/data already loaded in, you can see how that would upset your users if all there accounts were deleted. 

If you were to make the edits in the orginal create table migration file after its been migrated they wouldn't be implemented.

A good way to think of the schema.rb file is read only, do not edit, and do not delete.


	

