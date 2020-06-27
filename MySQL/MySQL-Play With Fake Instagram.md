# Play with thousands of fake instagram data
## Question # 1 - Five oldest users
> Description: We want to reward our users who have been around the longest.

> Translation: Find the 5 oldest users.
```sql
SELECT * FROM users ORDER BY created_at LIMIT 5;
```



## Question # 2 - Most popular registeration day
> Description: We need to figure out when to schedule an ad campgain. What day of the week do most users register on?

> Translation:  Most popular registeration day

```sql
-- #2 Most popular registeration day
SELECT
    COUNT(*) AS total,
    DAYNAME(created_at) day
FROM users
GROUP BY day
ORDER BY total DESC;
```



## Question # 3 - Identify Inactive Users
> Description:We want to target our inactive users with an email campaign.

> Translation: Find the users who have never posted a photo

```sql
-- 3. Identify Inactive Users (users with no photos)
SELECT username
FROM users
LEFT JOIN photos
    ON users.id = photos.user_id
WHERE photos.id IS NULL;
```



## Question # 4 - Find most liked photo and author
> Description: We're running a new contest to see who can get the most likes on a single photo.

> Translation: Found the single most liked photo and its' author

```sql
-- Found the single most liked photo and author
SELECT
    username,
    photos.id,
    photos.image_url,
    COUNT(*) AS total
FROM photos
INNER JOIN likes
    ON likes.photo_id = photos.id
INNER JOIN users
    ON photos.user_id = users.id
GROUP BY photos.id
ORDER BY total DESC
LIMIT 1;
```
#### Use max and subquery
```sql
SELECT
    username,
    photos.id,
    photos.image_url,
    COUNT(*) AS total
FROM photos
INNER JOIN likes
    ON likes.photo_id = photos.id
INNER JOIN users
    ON photos.user_id = users.id
GROUP BY photos.id
HAVING total =
	(SELECT MAX(a.Count_Likes)
        FROM
		(SELECT COUNT(*) AS Count_Likes
		FROM likes
			JOIN photos
				ON likes.photo_id = photos.id
		GROUP BY photos.id) AS a
        )
;
```

## Question # 5 - Avg number of photos per user
> Description: Our Investors want to know...How many times does the average user post?

> Translation: total photo num / total user number
```sql
-- #4 Calculate avg number of photos per user
SELECT
    (SELECT COUNT(*) FROM photos) / (SELECT COUNT(*) FROM users) as avg;
```


## Question # 6 - Top 5 most commonly used hashtage
> Description: A brand wants to know which hashtags to use in a post. What are the top 5 most commonly used hashtags?

> Translation: Top 5 most commonly used hashtags

```sql
-- #5 Top 5 most commonly used hashtage
SELECT
    tag_name,
    COUNT(*) AS freq
FROM tags
JOIN photo_tags
    ON photo_tags.tag_id = tags.id
GROUP BY tags.id
ORDER BY freq DESC
LIMIT 5;
```

## Question # 6
> Description: We have a small problem with bots on our site... Find users who have liked every single photo on the sit

> Translation: Find users who liked every single photo on the site

```sql
-- # Find users who liked every single photo on the site
SELECT
    users.id AS user_id,
    username,
    COUNT(*) AS num_likes
FROM users
INNER JOIN likes
    ON users.id = likes.user_id
GROUP BY users.id
HAVING num_likes = (SELECT COUNT(*) FROM photos);
```

> ## New grammar: HAVING
having replace where to constrants what rows to be displayed because where has to be before group by.
