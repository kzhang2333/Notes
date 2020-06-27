# Clone Instagram

## User
```sql
CREATE TABLE users (
    id INTEGER AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(255) UNIQUE NOT NULL,
    created_at TIMESTAMP DEFAULT NOW()
);
```
> Notice: username is **NOT NULL**
## Photos
```sql
CREATE TABLE photos (
    id INTEGER AUTO_INCREMENT PRIMARY KEY,
    image_url VARCHAR(255) NOT NULL,
    user_id INTEGER NOT NULL,
    created_at TIMESTAMP DEFAULT NOW(),
    FOREIGN KEY(user_id) REFERENCES users(id)
);
```
> Notice: image_url, user_id are **NOT NULL**
## Comments
```sql
CREATE TABLE comments (
    id INTEGER AUTO_INCREMENT PRIMARY KEY,
    comment_text VARCHAR(255) NOT NULL,
    photo_id INTEGER NOT NULL,
    user_id INTEGER NOT NULL,
    created_at TIMESTAMP DEFAULT NOW(),
    FOREIGN KEY(photo_id) REFERENCES photos(id),
    FOREIGN KEY(user_id) REFERENCES users(id)
);
```
> Notice: comment_text, photo_id, user_id are **NOT NULL**

## Likes
```sql
CREATE TABLE likes (
    user_id INTEGER NOT NULL,
    photo_id INTEGER NOT NULL,
    created_at TIMESTAMP DEFAULT NOW(),
    FOREIGN KEY(user_id) REFERENCES users(id),
    FOREIGN KEY(photo_id) REFERENCES photos(id),
    PRIMARY KEY(user_id, photo_id)
);
```
> Notice: Here use combination of (user_id, photo_id) as **PRIMARY KEY** to prevent multiple like of same user to same photo.

## Follows
```sql
CREATE TABLE follows (
    follower_id INTEGER NOT NULL,
    followee_id INTEGER NOT NULL,
    created_at TIMESTAMP DEFAULT NOW(),
    FOREIGN KEY(follower_id) REFERENCES users(id),
    FOREIGN KEY(followee_id) REFERENCES users(id),
    PRIMARY KEY(follower_id, followee_id)
);
```
> Notice: follows table describe relationship of follower and followee. Please notice it use combination of (follower_id, followee_id) as **PRIMARY KEY** to prevent same follows.

## Tags
### Three Solutions

1. One table. Include hashtags in photo table

2. Two tables. User table and hashtag table. One user have multiple rows in hashtag table to store different tags.

3. Three tables. User, Hashtag, User_Hashtag. User tables only store user info. Hashtag table only store hashtag info. User_Hashtag only store user_id and hashtag_id.

Solution 1 is fast but it limits info about tag and how many tags a photo can have. Also you need to be careful whiling searching.

Solution 2 supports unlimited number of tags. It is slower that solution 1. And it has duplicate info.

Solution 3 support unlimited tags and can store tag info. But it costs more when inserting and updating when new tags are created. And we have to care orphans.

**In words, there is *NO BEST SOLUTION!* the best practice is to use two versions in your system, first solution and third solution.
For common tags, we use the third solution. For rear tags, we use the first solution.**

### Our SOLUTION
```sql
CREATE TABLE tags (
  id INTEGER AUTO_INCREMENT PRIMARY KEY,
  tag_name VARCHAR(255) UNIQUE,
  created_at TIMESTAMP DEFAULT NOW()
);

CREATE TABLE photo_tags (
    photo_id INTEGER NOT NULL,
    tag_id INTEGER NOT NULL,
    FOREIGN KEY(photo_id) REFERENCES photos(id),
    FOREIGN KEY(tag_id) REFERENCES tags(id),
    PRIMARY KEY(photo_id, tag_id)
);
```
> Here we use third solution. `PRIMARY KEY(photo_id, tag_id)` is used to prevent duplication. 
