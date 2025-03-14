---
layout: post-with-tags
title:  "SQL windows are pretty useful"
date:   2025-03-08
tags:   database
---
<figure>
	<img src="{% link assets/windows.png %}" class="article-image" style="object-position: -175px center" />
	<figcaption>Émile-Antoine Bayard, illustration in Jules Verne's 'From the Earth to the Moon'</figcaption>
</figure>

Let's say we have a `posts` table:

```sql
CREATE TABLE posts (
    id INT NOT NULL AUTO_INCREMENT,
    user_id INT NOT NULL,
    likes INT,
    PRIMARY KEY(`ID`)
);

INSERT INTO posts (user_id, likes)
VALUES
(1, 7),  (1, 4), (1, 3),
(2, 12), (2, 5), (2, 1), (2, 20);
```

| id  | user_id | likes |
| --- | ------- | ----- |
| 1   | 1       | 7     |
| 2   | 1       | 4     |
| 3   | 1       | 3     |
| 4   | 2       | 12    |
| 5   | 2       | 5     |
| 6   | 2       | 1     |
| 7   | 2       | 20    |

We want to know each user's most liked post.
With the data we've provided, that would be:

| user_id | most_liked_post |
| ------- | --------------- |
| 1       | 1               |
| 2       | 7               |

Getting the **most likes** a user has had in a post is easy enough to do with a `GROUP BY`:

```sql
SELECT user_id, MAX(likes) AS most_likes
FROM posts
GROUP BY user_id;
```

| user_id | most_likes |
| ------- | ---------- |
| 1       | 7          |
| 2       | 20         |

But what we were interested in was **what post** (the post id) is the one that got the most likes.
As you probably know adding the post `id` to the `GROUP BY` is not going to help us:

```sql
SELECT user_id, MAX(likes) AS most_likes, id
FROM posts
GROUP BY user_id;
```
```
Expression #3 of SELECT list is not in GROUP BY clause and contains nonaggregated column 'mysql.posts.id' [...]
```

That's because we can't have columns in the `SELECT` statement which are not part of your `GROUP BY`. 

What we could do is use a `WINDOW` function.

## Using a window function

Window functions are functions applied to each row using data from other rows. They are typically used for things like running totals, moving averages...
I found [this article](https://notso.boringsql.com/posts/window-functions-introduction/) to be an excellent introduction.

It just so happens they are also useful in our particular case:

```sql
SELECT 
    DISTINCT user_id,
    FIRST_VALUE(id) OVER (PARTITION BY user_id ORDER BY likes DESC) AS most_liked_post
FROM posts;
```

| user_id | most_liked_post |
| ------- | --------------- |
| 1       | 1               |
| 2       | 7               |

Let's go over the query.

The `PARTITION BY user_id` part works like our previous `GROUP BY user_id`, but then we're also able to `ORDER BY likes` (which `GROUP BY` wouldn't allow because you have to `ORDER BY` the same columns that you `GROUP BY`).

We then use the `FIRST_VALUE()` function to get the `most_likes` and `most_liked_post`. `FIRST_VALUE()` exists as a `WINDOW` function, but not as an aggregate function (so you cannot use it with `GROUP BY`).

Finally, we use the `DISTINCT user_id` to get only one row per user. If we didn't we would get our results once per row of the table.

My examples are in MySQL but I've also written down how to use window functions in [PostgreSQL]({% link _posts/2025-03-08-postgresql-window-functions.markdown %}) and [OracleDB]({% link _posts/2025-03-08-oracle-window-functions.markdown %}).

## Extra things

We could modify our query to get more info, for example the least liked post and the number of likes of both the most and least liked post.

```sql
SELECT 
    DISTINCT user_id,
    FIRST_VALUE(id)    OVER likes_desc AS most_liked_post,
    FIRST_VALUE(likes) OVER likes_desc AS most_likes,
    FIRST_VALUE(id)    OVER likes_asc  AS least_liked_post,
    FIRST_VALUE(likes) OVER likes_asc  AS least_likes
FROM posts
WINDOW likes_desc AS (PARTITION BY user_id ORDER BY likes DESC),
       likes_asc  AS (PARTITION BY user_id ORDER BY likes ASC);
```

<div style="overflow-x: auto;">
	<table>
		<tr>
			<th>user_id</th>
			<th>most_liked_post</th>
			<th>most_likes</th>
			<th>least_liked_post</th>
			<th>least_likes</th>
		</tr>
		<tr>
			<td>1</td>
			<td>1</td>
			<td>7</td>
			<td>3</td>
			<td>3</td>
		</tr>
		<tr>
			<td>2</td>
			<td>7</td>
			<td>20</td>
			<td>6</td>
			<td>1</td>
		</tr>
	</table>
</div>

> ⚠️ Wait! Why don't you reuse the `WINDOW likes_desc` and then get the `least_liked_post` and `least_likes` using `LAST_VALUE()`?

Well, `LAST_VALUE()` does not operate on the whole window partition but rather on the current rows and the rows so far (`FIRST_VALUE()` also works this way, actually, but because of the `ORDER BY` it doesn't matter. You can read more [here](https://dev.mysql.com/doc/refman/8.4/en/window-functions-frames.html#:~:text=Aggregate%20functions%20used%20as%20window%20functions%20operate%20on%20rows%20in%20the%20current%20row%20frame,%20as%20do%20these%20nonaggregate%20window%20functions)).

Let's see what would happen if we used `LAST_VALUE()`.

```sql
SELECT
    user_id,
    FIRST_VALUE(id)    OVER likes_desc AS most_liked_post,
    FIRST_VALUE(likes) OVER likes_desc AS most_likes,
    LAST_VALUE(id)     OVER likes_desc AS least_liked_post,
    LAST_VALUE(likes)  OVER likes_desc AS least_likes
FROM posts
WINDOW likes_desc AS (PARTITION BY user_id ORDER BY likes DESC);
```

<div style="overflow-x: auto;">
	<table>
		<tr>
			<th>user_id</th>
			<th>most_liked_post</th>
			<th>most_likes</th>
			<th>least_liked_post</th>
			<th>least_likes</th>
		</tr>
		<tr>
			<td>1</td>
			<td>1</td>
			<td>7</td>
			<td>2</td>
			<td>7</td>
		</tr>
		<tr>
			<td>1</td>
			<td>1</td>
			<td>7</td>
			<td>2</td>
			<td>7</td>
		</tr>
		<tr>
			<td>1</td>
			<td>1</td>
			<td>7</td>
			<td>3</td>
			<td>3</td>
		</tr>
		<tr>
			<td>2</td>
			<td>7</td>
			<td>20</td>
			<td>7</td>
			<td>20</td>
		</tr>
		<tr>
			<td>2</td>
			<td>7</td>
			<td>20</td>
			<td>4</td>
			<td>2</td>
		</tr>
		<tr>
			<td>2</td>
			<td>7</td>
			<td>20</td>
			<td>5</td>
			<td>5</td>
		</tr>
		<tr>
			<td>2</td>
			<td>7</td>
			<td>20</td>
			<td>6</td>
			<td>1</td>
		</tr>
	</table>
</div>

I've removed the `DISTINCT` in the query so it's easy to observe that `least_liked_post` and `least_likes` is updated for every row we evaluate (`most_liked_post` and `most_likes` doesn't change because of the `ORDER BY likes DESC`).