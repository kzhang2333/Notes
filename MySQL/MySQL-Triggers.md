 # MySQL - Triggers
 ## Defination
 SQL statements that are AUTOMATICALLY RUN ("triggered") when a specific table is changesed.

 ## Syntax
 ### Code
 ```sql
 CREATE TRIGGER trigger_name
    trigger_time trigger_event ON table_name FOR EACH ROW
    BEGIN
    ...
    END;
  ```
  ### Three Important Variables in codes
| trigger_time | trigger_event | table_name |
| ------------ | ------------- | ---------- |
| BEFORE       | INSERT        | photos     |
| AFTER        | INSERT        | users      |
|              | DELETE        |            |

### Use case
- Data validation
- Manipulating other tables

#### Example#1 - user age validation
```sql
DELIMITER $$

CREATE TRIGGER must_be_adult
     BEFORE INSERT ON users FOR EACH ROW
     BEGIN
          IF NEW.age < 18
          THEN
              SIGNAL SQLSTATE '45000'
                    SET MESSAGE_TEXT = 'Must be an adult!';
          END IF;
     END;
$$

DELIMITER ;
```
In this example
- "BEFORE" is trigger_time
- "INSERT" is trigger_event
- "users" is table_name
- "NEW" is data aht is about to be inserted
- "DELIMITER $$" - 改变delimiter 为"$$", 注意最后必须改回";"


MySQL Error Message
1. A numeric error code (1146). This number is  **MySQL-specific**
2. A five-character SQLSTATE value ('42S02').  **ALL SQL Language standardized**
The values are taken from ANSI SQL and ODBC and are more standardized.
3. A message string - textual description of the error


#### Example#2 - prevent self-follow
```sql
DELIMITER $$

CREATE TRIGGER example_cannot_follow_self
     BEFORE INSERT ON follows FOR EACH ROW
     BEGIN
          IF NEW.follower_id = NEW.following_id
          THEN
               SIGNAL SQLSTATE '45000'
                    SET MESSAGE_TEXT = 'Cannot follow yourself, silly';
          END IF;
     END;
$$

DELIMITER ;
```

#### Example#3 - log unfollow action
Idea is to log unfollowing actions whenevner **one row in follows table is deleted**
```sql
DELIMITER $$

CREATE TRIGGER create_unfollow
    AFTER DELETE ON follows FOR EACH ROW
BEGIN
    INSERT INTO unfollows
    SET follower_id = OLD.follower_id,
        followee_id = OLD.followee_id;
END$$

DELIMITER ;
```

### Managing and A Warning
1. `Show Triggers` - display Triggers
2. `DROP TRIGGER trigger_name;` - removing Triggers

> # WARNING - Too mnay triggers can manke debuging difficult because triggers are hidden
