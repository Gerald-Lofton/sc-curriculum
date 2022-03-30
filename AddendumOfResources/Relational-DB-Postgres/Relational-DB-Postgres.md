# Relational Database - `PostgreSQL`

## Relational Data

In a previouse session, we used `express` to build on our `lowdb`-based blog post API. While `lowdb` works for the simplest cases, it does not do well with _relational data_.

Relational data is a way of modeling data through its relationships, organizing entity categories into a single structure (often "tables") with many properties ("columns") and instances of that entity ("rows") that is related to other entities through unique identifiers.

This is a similar model to spreadsheets or other forms of columnar data presentation. By building our persistent data stores (or "databases") around this relational model, we can then enforce guarantees about those relationships, and we can query for subsets of entities through their relationships with one another more easily.

<br>

### **Relational DB Example**

That's all a bit abstract, so let's take a look at a more concrete example using our existing `/posts` resource.

Every post is unique, and there are a number of properties that we could add that would continue to be unique to that blog post, e.g.:

```javascript
{
  "title": "Post Number 1",
  "body": "This is a blog post",
  "id": "79fa42ee-1c92-4a34-b55a-d37b0a8bc2e0",
  "published": 1597592298458
}
```

But what about including information about the author of a post? Let's include the `authorName` in each post, too:

```javascript
[
  {
    "authorName": "Test Testerson"
    "title": "Post Number 1",
    "body": "This is a blog post",
    "id": "79fa42ee-1c92-4a34-b55a-d37b0a8bc2e0",
    "published": 1597592298458
  },
  {
    "authorName": "Test Testerson",
    "title": "Post Number 2",
    "body": "This is a blog post",
    "id": "89fa42ee-1c92-4a34-b55a-d37bsldflk8c",
    "published": 1597592298458
  },
]
```

While this works, there's a **problem**:

We probably want to include a lot more information about an Author than their name. What if they need to log in?
Do they get a username and a password?
What about an email address, or a _nom de plume_ versus a legal name?

While we could include all of that information in every post that author contributes, maintaining that ever-changing collection of information inside the `post` entity would be a nightmare. Instead, we'd probably want to include a separate `author` resource that is then **referenced** in the `post` resource. Something like:

```javascript
{
  "posts": [
    {
      "authorId": "79fee662u-1c92-4a34-b55a-d37675ikkC"
      "title": "Post Number 1",
      "body": "This is a blog post",
      "id": "79fa42ee-1c92-4a34-b55a-d37b0a8bc2e0",
      "published": 1597592298458
    },
    {
      "authorId": "79fee662u-1c92-4a34-b55a-d37675ikkC"
      "title": "Post Number 2",
      "body": "This is a blog post",
      "id": "89fa42ee-1c92-4a34-b55a-d37bsldflk8c",
      "published": 1597592298460
    },
  ],
  "authors": [
    {
      "id": "79fee662u-1c92-4a34-b55a-d37675ikkC",
      "name": "Test Testerson",
      "email": "test@testerson.com"
    }
  ]
}
```

We now have a relationship between _posts_ and _authors_. This is a good thing! Now we can make sure that author records can be as rich as they need to be without affecting the ergonomics of posts, and we can better adhere to the RESTful principle of Unique Resource Identifiers by exposing separate `posts` and `authors` endpoints. Let's re-organize our API to include an `authors` resource as well!

<br>

### **Constraints and Validation**

Our simple database works well enough for simple cases and small prototypes, but there's a problem: how do we enforce the consistency of our data? What do you think will happen in the following cases?

1. What if we set the `authorId` of a post to `null`? Does that make sense?
2. What happens if we set the `authorId` to an author that doesn't exist?
3. What happens if we _delete_ an author with posts (leaving the posts' authorId in place)?

All of these are cases where the data model is invalidated by changing requirements. The burden of maintaining up-to-date data then becomes the burden of the application programmers (which doesn't make much sense).

Instead, we can offload that maintenance burden to our database software through the use of _**constraints**_.

Using a robust relational database, we can make sure that all of the above cases are _impossible_. And by making those cases impossible, it becomes much easier to query our data quickly (since we can assume complete and accurate data for all of the above cases).

<br>

---

<br>

## Postgres & SQL Commands

<br>

### **Installing PostgreSQL**

The relational database that we'll be examining here is called **Postgres**. Let's give it a look!

Before we begin, make sure that you've [downloaded Postgres for your operating system](https://www.postgresql.org/download/) and installed it for your operating system.

> **TIP**: Make sure you pick a password you can remember for the default database or you'll have to uninstall and re-install Postgres!

<br>

Postgres works as a "_daemonized_" process. This means that it's always running in the background on a particular port. In the case of Postgres, it's almost always `5432`. To check that the `postgres` daemon is running, enter the following in your terminal:

```bash
psql -U postgres -d postgres
```

> **NOTE**: The `-U` flag specifies the user, and the `-d` flag specifies the database. In this case, we are using the default user (`postgres`) as well as the default database (also called `postgres`).

<br>

> **Windows Users**: If you get an error like `winpty: error: cannot start 'psql.exe': Not found in PATH` follow the [instructions in this link](https://sqlbackupandftp.com/blog/setting-windows-path-for-postgres-tools) (your _version_ of Postgres may differ from the version listed in the link)

<br>

This should open up the **postgres REPL** for making queries against the default database with a default user. You can inspect your existing database _tables_ (the `postgres` term for a resource type like `posts` or `authors`) with the shortcut `\d`. You should get back a response that says 'Did not find any relations'.

<br>

## Postgres SQL Commands

<br>

## `CREATE TABLE`

So now that we have an empty database, let's create some tables. To do that, we need to use the `CREATE TABLE` SQL command, which has the following syntax:

```sql
CREATE TABLE tableName (
  id SERIAL PRIMARY KEY
  column1 <data type> <constraints>
  column2 <data type> <constraints>
  ...
);
```

For our posts and authors tables, we will use the following commands to create two tables with a set of _columns_.:

> **TIP**: Don't forget the `;` at the end of your command!
>
> The Postgres REPL also has a problem with `"`, so use `'` for strings

```sql
CREATE TABLE authors (
  id SERIAL PRIMARY KEY,
  name TEXT NOT NULL,
  email TEXT NOT NULL
);

CREATE TABLE posts (
  id SERIAL PRIMARY KEY,
  title TEXT NOT NULL,
  body TEXT NOT NULL,
  author_id INTEGER REFERENCES authors NOT NULL
);
```

Think of columns like you would in a spreadsheet, but, in this case, **validated** by _types_ and **constrained** by _null checks_. These validations and constraints make it impossible to do things like delete an author if they still have posts, or create a post without a title, or create a post with an `author_id` that doesn't exist.

<br>

## Read All with `SELECT *`

Recall that we had previously mapped CRUD operations to HTTP verbs (where CREATE is POST or PUT, READ is GET, UPDATE is PATCH or PUT, and DELETE is DELETE). We have similar commands in SQL (and, thus, in Postgres). Let's start by "reading" our entire authors table with `SELECT`. Try the following:

```sql
SELECT * FROM authors;
```

`SELECT` is the "read" command in SQL, and `*` is like a global pattern that means "select _all_ of the columns" `FROM` this table. This is called a "query". You'll notice that this query returned zero _rows_... which makes sense, since there are no authors in our database.

<br>

## Create with `INSERT INTO`

Let's create an author with `INSERT`. `INSERT` syntax looks like this:

```sql
INSERT INTO <table> (<column1>, <column2>, <column3>) VALUES ('<value for column1>', '<value for column2>', '<value for column3>')
```

We can insert an entry into our `authors` table like this:

```sql
INSERT INTO authors (name, email) VALUES ('Test Testerson', 'test@test.com');
```

Notice that we need to explicitly map _columns_ to _values_. All text in SQL is surrounded by quotation marks (like we see with 'Test Testerson'), so we know that that these values satisfy the `TEXT` type requirements of the `name` and `email` columns.

You'll also notice that we didn't need to include `id`; this is because `id` is of type `SERIAL`, which is an auto-incrementing counter of integer values that's generated for us whenever a new `author` row is inserted into the table.

Try the previous `SELECT` query again to see what happens now! Be sure to note the `id` that comes back from that query.

<br>

## Update with `UPDATE`

Imagine for a moment that we wanted to update that author's email address. How might we do it? The SQL command here is called `UPDATE`, and its syntax looks like this:

```sql
UPDATE <table> SET <column> = <new value> WHERE id = <entry ID>
```

We can update our author entry by using the ID we saw from our previous `SELECT` statement.

```sql
UPDATE authors SET email = 'new.email@test.com' WHERE id = <entry ID>;
```

...where `<entry ID>` is the id of the newly-created author. Notice that you can't possibly know that id ahead-of-time (even if it's easy to predict in this case).

The `WHERE` clause is important in this case, as we only want to `UPDATE` a single row. Think of `WHERE` like a kind of `filter()` on a collection: the query is only evaluated for those rows that pass the condition (or conditions) listed in the query's associated `WHERE` clause. A common pattern is the use of a unique `id` to "filter" possible rows down to a single row.

<br>

## Delete with `DELETE`

Similar to `UPDATE` we can also `DELETE` a single row with the following syntax:

```sql
DELETE FROM authors WHERE id = <entry ID>;
```

...again using the original `author` id.

<br>

---

<br>

### Activity 1 (Students): Add Entries to Our Tables

Let's add some data to our tables

1. Use `psql` to add at least one author back to the `author` table. Notice how the original `id` value is never repeated!

2. Additionally, make sure that you've created some posts with a similar `INSERT` statement. Notice that the `author_id` _must_ be a valid id in the `authors` table! If not, then `psql` prevents `INSERT` in order to preserve our data's completeness and accuracy.

<br>

---

<br>

## `Node-Postgres` and Connection Pools

So now we have data in our local postgres database, but how can we actually _use_ that data in Node? To do so, we'll use the [`node-postgres`](https://node-postgres.com/) (or `pg`) node module. Before we do that, though, we need to learn a bit more about database connections.

Recall that RESTful APIs could _not_ be "stateful": every request had to carry any "state" around in its headers or body, and every "response" had to do the same.

Databases, though, are not RESTful. They carry state with a series of requests (i.e. "queries", as we've seen so far) through a single connection, and limit the number of active connections that a database can have at one time.

This poses a problem for a public API: if you have to reconnect to a database for every single request, those initial connections will be slow (and resource-intensive), and most active APIs will start requesting more connections than Postgres is often capable of providing.

The solution, then, is to use a **connection pool** like the one provided by `pg`. Connection pools start a few connections, then share them through a single Object in a Node process. This means that database-hitting requests are both faster and less taxing on the database itself. As a bonus, most of these connection pools provide a simple way of passing around a database client that can be used to make these queries.

Let's see how that will work with `node-postgres` and our `express` API.

<br>

---

<br>

### Activity 2 (Everyone): `express` and `pg`

1. Start by installing `pg` with `npm install pg`

2. Next, create a `db.js` file next to `index.js` in `server`. This will contain our database configuration.

3. The connection pool provided by `pg` needs to know how to connect to our local database. So let's provide a _connection string_ either through a `DATABASE_URL` environment variable (used with Heroku) or with a hard-coded value in `db.js`, like so:

   ```javascript
   const connectionString =
     process.env.DATABASE_URL || "postgres://postgres@postgres:5432";
   ```

   That connection string is a little complex, but it contains a lot of useful information. First, JS will look for an environment variable named `DATABASE_URL`. If it fails to find that environment variable, it sets the connection string to the hard-coded string. `postgres://` is the protocol, similar to `http://` in the browser. The first `postgres` is the user (same as the `-U` flag in `psql`) and the `postgres` after the `@` is the database (same as the `-d` flag in `psql`). As mentioned before, `5432` is the default port for Postgres.

4. Now we can create a connection pool for our `app`, like so:

   ```javascript
   const { Pool } = require("pg");

   const connectionString =
     process.env.DATABASE_URL || "postgres://postgres@postgres:5432";
   const pool = new Pool({ connectionString });

   module.exports = pool;
   ```

5. Let's try using postgres to `GET` all of our posts instead of using our `lowdb` database! In `/routers/posts.js`, try the following in the `/` route:

   ```javascript
   // all previous imports the same
   const db = require("../db");

   router.route("/posts").get((_request, response) => {
     // notice the callback pattern!
     db.query("SELECT * FROM posts", (error, posts) => {
       if (error) {
         // send the SQL error if something goes wrong
         response.status(500).json({ error });
       } else {
         response.json(posts);
       }
     });
   });
   ```

   Note the Postgres `query` syntax:

   ```js
   db.query("<SQL query string>", function callback(
     sqlErrorMessage,
     queriedEntries
   ) {
     // logic handling the error or entries
     // the error handling structure above is a good start
   });
   ```

6. Finally, see if we can refactor every route with the Postgres version instead of the `lowdb` version of our CRUD operations. Those queries should be identical to the ones that we used in `psql`!

<br>

---

<br>

## Heroku Deployment

So far, all of our work has been done _locally_. This is common for development, but not very useful for APIs that we want to share with others. You'll recall that our single-page app has both a local and a public version (called "production") which both track the `master` branch of your project's `git` repository.

We used Netlify to deploy the public version of our single-page-app, but Netlify is built to serve primarily static content (like HTML, CSS, and JavaScript files) in what's called a "serverless" environment. Serverless environments do not support the kind of long-running services that we've just written; as you might expect, it's difficult to deploy a "server" to a platform that is explicitly "serverless"\*.

To host our `express` server, we'll use a another platform for deploying `git`-based long-running processes called [Heroku](https://www.heroku.com/). Heroku also has the benefit of a Postgres plugin that will make it easier to use a relational database for our blog posts.

Let's walk through deploying our API to Heroku so that we can share our blog posts with the world!

<br>

---

<br>

### Activity 3: Deploying to Heroku

1. Make sure that you've signed up for a free Heroku account first! This will require email confirmation.

2. On the home page once you've logged in, click the `Create a new app` button. And choose a new app name.

3. Once you've chosen a name, pick the GitHub deployment method on the next screen. Just like Netlify, you should be able to search through a list of your repositories for automatic deployment. Select your SPA project, then click `Enable Automatic Deploys`.

4. Heroku looks for a `start` script in your `package.json` to decide how to run your `express` server. So in your `package.json`, add the following to your `scripts`:

   ```bash
   "start": "node ./server/app.js"
   ```

5. Once you stage and commit those changes, you should be able to watch the deployment process happen from the Overview dashboard before clicking the Open App button to see your API in action.

6. To see your request logs in real time, you can visit `https://dashboard.heroku.com/apps/YOUR-APP-NAME/logs`

<br>

---

<br>

### Activity 4: Postgres on Heroku

Last but not least, we need to get a running version of Postgres with a similar schema up-and-running in our production environment. Heroku has a Postgres add-on that makes this very easy for small projects like ours!

1. To start, install the `heroku` command-line interface by copying this into your terminal:

   ```bash
   curl https://cli-assets.heroku.com/install.sh | sh
   ```

   > **Windows Users**: Use [this link](https://devcenter.heroku.com/articles/heroku-cli#download-and-install) to download and install the Heroku CLI

   Then login with `heroku login`. Once logged-in, you can exit to your terminal with `Ctrl+C`.

2. Once the CLI is installed, go to your Heroku app dashboard and click on the 'Resources' tab. In that tab, there's a search bar in the Add-Ons section. Type in `postgres` and select the `Heroku Postgres` option that appears.

3. After selecting Heroku Postgres, you should be able to see a new dashboard link appear for this resource as soon as it's ready.

4. Now that your database exists, you can `psql` into that database with `heroku pg:psql -a <your heroku app name>`. Use your Heroku app name, not the entire url. This is similar to the `psql` we use locally, but is connected to your production database... so be careful!

   > **NOTE**: Another key difference between the `psql` REPL and the `heroku pg:psql` REPL is the syntax for columns in an `INSERT` or `UPDATE` statement
   >
   > `psql` syntax (no quotes around columns)
   >
   > ```sql
   > INSERT INTO posts (title, body) VALUES ('my title', 'my body');
   > ```
   >
   > `heroku psql` syntax (double quotes around columns)
   >
   > ```sql
   > INSERT INTO posts ("title", "body") VALUES ('my title', 'my body');
   > ```

5. Once you have a `psql` connection, you'll notice that your production database is just as empty as our local database was. Use the exact same table-creation commands that we used before to create `posts` and `authors` tables in your new database and to create a few posts and authors.

6. Once that is set up, push your project to `master` and watch Heroku handle your new postgres-backed deployment! Visit your API route on your Heroku link once your server is built to see your data.

7. At this point, you can replace any `axios` calls to `jsonplaceholder.typicode.com` in your SPA with your new public API.

   > **TIP:** If you run into a CORS Error where you cannot retrieve your data, examine the [corsHeader middleware module](./8.3-Example/corsHeader.js) and add it to your SPA's server.

8. So far we have only used `axios.get` to use the `GET` method, but we can use other HTTP methods with `axios`. See the [axios documentation](https://github.com/axios/axios#request-method-aliases) for more info.
