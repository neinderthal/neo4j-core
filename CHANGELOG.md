# Change Log
All notable changes to this project will be documented in this file.
This file should follow the standards specified on [http://keepachangelog.com/]
This project adheres to [Semantic Versioning](http://semver.org/).

## [Unreleased][unreleased]
- Nothing yet, placeholder

## [5.0.0] - 2015-05-20

### Changed
- Ruby 2.0.0 now required (>= 2.2.1 is recommended)
- Rake tasks `neo4j:(install|start|stop|restart|info|reset_yes_i_am_sure)` now output log messages / errors
- In `Query` chains, a `with` followed immediately by a `limit` and/or an `order` will have the `limit`/`order` clauses applied to it as you would expect
- Major refactoring using `rubocop` and speed improvements
- Queries are retried on failure to deal with `RWLock` errors

### Fixed
- Bug when starting/stopping embedded sessions repeatedly (like for tests) fixed

### Added
- `Query#count` method now available
- `Query#clause?` method now available to determine if a `Query` object has a particular clause defined
- Arrays can now be passed as labels in a `Query` chain (e.g. `.match(n: [:Person, "Animal"])` generates: MATCH (n:`Person`:`Animal`) )
- The `Query#set` and `Query#remove` methods now support setting labels (either via `Symbol`s or `Array`s)

**Changes above this point should conform to [http://keepachangelog.com/]**

## [4.1.0]
* A lot of work working with rubocop to clean up code
* Many instances of using strings were changed to use symbols
* Certain query responses are now automatically retried
* `Neo4j::Core::Query` changes
* WHERE clauses from `Query` chains are now surrounded by parentheses to avoid problems
* WHERE clauses now support Arrays and `nil` values for labels
* WHERE clauses with strings now support a second argument for parameters.  Examples:
  * `where("foo = {bar}", bar: value)`)
  * `where("foo = ?", value)`)
* REMOVE clauses now quote labels and support arrays to specify all labels
and properties for one variable
* RETURN clauses now turn `:neo_id` into `ID(var)`
* New method `#clause?` lets you determine if a `Query` object had a
clause called upon it in the past
* New method `#count` lets you query a count of a variable


## [4.0.0]
This release focuses mostly on performance and security.
* Fixed a few n+1 queries, force the use of params in more common locations.
* The CypherTransaction class was heavily refactored to improve the number of database connections required per transaction. This may be considered a breaking API change. Using `Neo4j::Transaction.new` or `Neo4j::Transaction.run` as instructed in all docs and examples protects the user from this, so there should be no changes required to old code in most cases.
* New Rake task: `neo4j:start_no_wait` -- thanks, @telzul!
* Massive refactoring and general cleanup by Brian.

## [3.1.1]
* Force more basic, common queries to use params. Early benchmarks suggest big performance improvements.
* Auth improvements. You can now use a valid Neo4j token with any username to authenticate.

## [3.1.0]
* Swapped out `START n=node...` for `MATCH (n) WHERE ID(n)...` for compatibility with Neo4j 2.2.
* Added a new class, CypherAuthentication, to support Neo4j 2.2's new authentication endpoint.
* Modified the `neo4j:install` rake task to disable authentication in Neo4j 2.2.
* New rake tasks that do exactly what you expect: `neo4j:enable_auth`, `neo4j:disable_auth`, `neo4j:change_password`. All 2.2 only, of course.
* Travis-CI will run against its MRI specs against Neo4j 2.2 with Ruby 2.1.5.
* Use params for all `create` actions to improve performance and security.
* More tweaks to the string escaping process.
* Mild refactoring for DRY and performance.

## [3.0.8]
* Small bugfix. Releasing so neo4jrb/neo4j doesn't need to pull from master in its new release.

## [3.0.7]
* Move improved escaping of Cypher params

## [3.0.6]
* When using Neo4j >= 2.1.5, use the metadata keys in Cypher responses to find labels when loading nodes
* Adds `version` method to Embedded and Server sessions
* Improved escaping of Cypher params

## [3.0.5]
* Bug fix: automatic parsing of basic auth params in URL (Thanks, Miha Rekar!)
* Adds find_in_batches method, see github wiki for documentation: https://github.com/neo4jrb/neo4j-core/wiki

## [3.0.4]
* Bug fixes to sanitize params and remove blank clauses

## [3.0.3]
* Adds a user agent string to connections to identify the driver with the server

## [3.0.2]
* Improved detection of transaction responses to prevent conflicts with property names

## [3.0.1]
* Improved handling of cypher responses, particularly within transactions

## [3.0.0]
No changes from rc 5

== 3.0.0.rc.5
* Misc fixes
* Changes to support neo4j gem
* Using faraday gem with net-http-persistent instead of httparty

== 3.0.0.rc.4
* Remove dependency on oj gem pending further tests.

== 3.0.0.rc.3
* Minor gemspec fix

== 3.0.0.rc.2
* Bugfix in gemspec related to oj and JRuby

== 3.0.0.rc.1
* Use JSON oj implementation for better performance on neo4j server
* Some performance improvements on regexps
* Better support for cypher collect to return array
* Security: prevent cypher injection
* Support to install via rake different neo4j server environments
* Support for query logger
* Distinct support for query dsl

== 3.0.0.alpha.19
* Better support for (nested) transaction #94
* Upgrade to Rspec 3 (#93 @ausmarton)
* Performance improvements for Neo4j Server, cache props (#86, #90, #91, chris, brian)

== 3.0.0.alpha.18
* Fix handling of arrays in embedded mode (#89)
* Performance improvements #86

== 3.0.0.alpha.17
* Complete rewrite of the query api (Brian Underwood#85)
* Better performance for Embedded Db - single ExecutionEngine instance in embedded DB (#83 chris)
* Added better error handling when trying to install Neo4j which does not exist via Rake
* Added better error handling when user forgot to create a session

== 3.0.0.alpha.16
* Impl rel_type for Neo4j::Relationship

== 3.0.0.alpha.15
* Prepared for RSpec 3.x
* Bumped neo4j-community to 2.1.1

== 3.0.0.alpha.14
* Improved and moved Neo4j::Label.query to Neo4j::Session.query (thanks Brian Underwood, Mark Bao)
* Implemented inspect method for some neo4j-server classes to make PRY/IRB happy.

== 3.0.0.alpha.13
* Fixing the neo4j:install rake task for systems without wget and Windows (#64, @ausmarton)
* Support for adding labels on existing nodes closes (#63)
* Session#query returns a hash for all values in a row (#61, @fiddur)

== 3.0.0.alpha.12
* Fixing find_nodes to not quote numeric values (#48)

== 3.0.0.alpha.11
* Added Basic Auth (HTTParty config) configuration on session (#58)
* Drop nil value on create (#56)

== 3.0.0.alpha.10
* Fix of data url in cypher session (#55)
* add escape sequence sanitization to cypher translator (#53)
* Handle update_props with nil value (#46)

== 3.0.0.alpha.9
* Support for RegExp search (#45)

== 3.0.0.alpha.8
* Support for Schema Constraints (#44)
* Fix for Rake Neo4j task (#43)

== 3.0.0.alpha.7
* Support for named sessions (#40, alex)
* Added method #update_props for Relationship and Node
* Added Neo4j::Relationship.create method

== 3.0.0.alpha.6
* Fixes for wrapper method on nodes needed by the neo4j 3.0 gem

== 3.0.0.alpha.5
* Better support for wrapping nodes, see Neo4j::Node::Wrapper

== 3.0.0.alpha.4
* Fix for cypher query, the wrapper hook method must be called to make neo4j gem happy

== 3.0.0.alpha.3
* Fix for cypher query where the column values was wrong

== 3.0.0.alpha.2
* Support for Neo4j 2.0.0 and the neo4j-community jar, removed includedd JAR files
* Simple Event support, notifying listener when database is started/usable
* Fix requirement for standalone neo4j-core Gem (#34, Kevin Hall)

== 3.0.0.alpha.1
* First test

== 2.3.0 / 2013-06-18
* Use 1.9 Neo4j Jars, (#29, Jannis)
* added ability to pass in params to Neo4j._query method (#28, kmussel)

== 2.2.4 / 2013-05-19
* Fix for from.rels(...).to_other(to), #27
* Add support for Relationship#nodes accessor #25, David Butler
* Fix for NoMethodError on exception message for the []= property method, #24, Aish Aishfenton
* Made Neo4j.start threadsafe #23, David Butler
* Better logging for Java Exception #22, David Butler
* Fixed RSpec problems and make it compatible with future RSpec 3

== 2.2.3 / 2012-12-28
* Raise an exception if get_or_create is called inside a transaction
* fix for JRuby 1.8 mode, #20

== 2.2.2 / 2012-12-27
* Use Neo4j 1.8.1 and avoid JRuby Warnings #19

== 2.2.1 / 2012-12-17
* Fix for create nodes and relationship using Cypher #17
* Fix for JRuby 1.7.1 - don't impl eql and == #18

== 2.2.0 / 2012-10-02
* Use 1.0.0 of neo4j-cypher
* Fix of Neo4j::Config issue using boolean values, andreasronge/neo4j#218
* The []= operator checks that the value is a valid Neo4j value #16

== 2.2.0.rc1 / 2012-09-21
* Deleted, refactored, improved and moved cypher stuff to neo4j-cypher gem
* Add neo4j statistics, #15

== 2.1.0 / 2012-08-14

* Fix for cypher query with node(*) throw error. #13
* Add methods for Neo4j HA: ha_enabled? ha_master? #12
* Upgrade to 1.8.M06 - breaking changes, some traversals methods use path objects

== 2.0.1 / 2012-06-07

* Remove hard coded Gem dependencies to neo4j-advanced and neo4j-enterprise, #11
* Make it possible to specify protected keys for Neo4j::Node.update, #8, #9
* Added missing method for start and end_node, #7

== 2.0.0 / 2012-06-05