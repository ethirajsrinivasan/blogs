## Use of Includes

Includes is a handy method to do eager loading on your associations as stated in the rails documentation - "With includes, Active Record ensures that all of the specified associations are loaded using the minimum possible number of queries."

Lets look at some of the ways to use includes

1. Just plain `includes` statement

```ruby 
users = User.includes(:url_shortner_logs)
```
```shell
User Load (0.8ms)  SELECT "users".* FROM "users"
UrlShortnerLog Load (1.2ms)  SELECT "url_shortner_logs".* FROM "url_shortner_logs" WHERE "url_shortner_logs"."user_id" IN (2, 3, 4, 1)
```

It fires two queries and loads User and UrlShortnerLog tables in memory.
```ruby
users.first.url_shortner_logs
```
The above command does not make a DB call instead loads the data from memory

2. `Includes` with `where`

```ruby
UrlShortner.includes(:url_shortner_logs).where(url_shortner_logs: {created_at: Date.today})
```
```shell
SQL (1.2ms)  SELECT "url_shortners"."id" AS t0_r0, "url_shortners"."short_url" AS t0_r1, "url_shortners"."original_url" AS t0_r2, "url_shortners"."sanitized_url" AS t0_r3, "url_shortners"."created_at" AS t0_r4, "url_shortners"."updated_at" AS t0_r5, "url_shortner_logs"."id" AS t1_r0, "url_shortner_logs"."url_shortner_id" AS t1_r1, "url_shortner_logs"."user_id" AS t1_r2, "url_shortner_logs"."browser" AS t1_r3, "url_shortner_logs"."version" AS t1_r4, "url_shortner_logs"."platform" AS t1_r5, "url_shortner_logs"."created_at" AS t1_r6, "url_shortner_logs"."updated_at" AS t1_r7 FROM "url_shortners" LEFT OUTER JOIN "url_shortner_logs" ON "url_shortner_logs"."url_shortner_id" = "url_shortners"."id" WHERE "url_shortner_logs"."created_at" = $1  [["created_at", "2017-07-30"]]
```

The above query makes a single DB call and does left outer join on the where conditions.

3. `Includes` with `where` and `references`
```ruby
UrlShortner.includes(:url_shortner_logs).where("url_shortner_logs.created_at= '2017–07–30'")
```
This query will throw ActiveRecord::StatementInvalid: PG::UndefinedTable: ERROR. It does not have reference to the table UrlShortnerLogs
To solve this, rails provides a method called references and it works only in conjunction with includes
```ruby
UrlShortner.includes(:url_shortner_logs).where("url_shortner_logs.created_at= '2017-07-30'").references(:url_shortner_logs)
```
```shell
SQL (0.5ms)  SELECT "url_shortners"."id" AS t0_r0, "url_shortners"."short_url" AS t0_r1, "url_shortners"."original_url" AS t0_r2, "url_shortners"."sanitized_url" AS t0_r3, "url_shortners"."created_at" AS t0_r4, "url_shortners"."updated_at" AS t0_r5, "url_shortner_logs"."id" AS t1_r0, "url_shortner_logs"."url_shortner_id" AS t1_r1, "url_shortner_logs"."user_id" AS t1_r2, "url_shortner_logs"."browser" AS t1_r3, "url_shortner_logs"."version" AS t1_r4, "url_shortner_logs"."platform" AS t1_r5, "url_shortner_logs"."created_at" AS t1_r6, "url_shortner_logs"."updated_at" AS t1_r7 FROM "url_shortners" LEFT OUTER JOIN "url_shortner_logs" ON "url_shortner_logs"."url_shortner_id" = "url_shortners"."id" WHERE (url_shortner_logs.created_at= '2017-07-30')
```
We can also observe that includes makes two DB calls in basic query condition and makes a single DB call when where clause is being used.
