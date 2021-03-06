Lazy Susan
==========

#### Overview
Lazy Susan is a rails generator that looks for tables in your database that are missing indexes, and crates a Rails migration to automatically fix the issue. Lazy Susan thinks that any "id" column in your database, as well as any column whose name ends in "_id", ought to have an index on it. Additionally, if Lazy Susan finds a table with exactly two columns ending in "_id", she suspects that she's looking at a join table, and figures that the join ought to be indexed both ways.

#### Stability Notice
Lazy Susan is currently considered a beta project. It doesn't do anything destructive, but it's probably worth checking out the generated migrations to make sure the resulting migration is one that you're comfortable running on your database.

#### Operation
In order to use Lazy Susan, you need to have your development database up migrated up; the gem works by inspecting your database through ActiveRecord. Once that's taken care of, simply list the gem in your gemfile, and run `rails g lazy_susan`. She'll take care of the rest.

Technical Details
-----------------

#### Migration Naming
Lazy Susan creates a Rails migration with the familiar timestamp. In order to minimize naming collisions, the migration is named "LazySusanForXxxxx", where Xxxxx is the most recent table getting a new index. 

#### Join Tables
In addition to indexing all id / _id columns, Lazy Susan tries to detect join tables. If you have a table, UserAddresses, that has columns "id", "user_id", and "address_id", Lazy Susan will pick up on the two _id columns and identify UserAddresses as a join table. In addition to creating indexes on user_id and address_id separately, she will also create indexes for [:user_id, :address_id] and [:address_id, :user_id], to maximize efficiency of lookup in both directions.