## Adjancency List
### Intro
1. Native solution nearly everyone use.  
2. Each entry knows its immediate parent.  

Hirerachical Data  
![](https://i.imgur.com/KpVeWoE.png)

Comments Table  
![](https://i.imgur.com/Cp9yJiT.png)

```cmd
CREATE TABLE `comments` (
  `comment_id` int(11) unsigned NOT NULL AUTO_INCREMENT,
  `parent_id` int(11) DEFAULT NULL,
  `author` varchar(20) NOT NULL DEFAULT '',
  `comment` varchar(255) NOT NULL DEFAULT '',
  PRIMARY KEY (`comment_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;


INSERT INTO `comments` (`comment_id`, `parent_id`, `author`, `comment`)
VALUES
	(1,NULL,'Fran','What the cause of this bug?'),
	(2,1,'Ollie','I think it is a null pointer.'),
	(3,2,'Fran','No, I checked for that.'),
	(4,1,'Kukla','We need to check valid input.'),
	(5,4,'Ollie','Yes, that is a bug.'),
	(6,4,'Fran','Yes, please add a check'),
	(7,6,'Kukla','That fixed it.');
```
### Insert New Node
```cmd
INSERT INTO comments (parent_id, author, comment) VALUES (5, 'Fran', 'I agree!');
```

![](https://i.imgur.com/SfmJn2j.png)
![](https://i.imgur.com/UIuqnQY.png)

### Move Node or Subtree
```cmd
UPDATE comments SET parent_id = 3 WHERE comment_id = 6;
```

![](https://i.imgur.com/I3Bav38.png)
![](https://i.imgur.com/cyzhiWQ.png)

### Query Immediate Child/Parent
1. Query a node's children
```cmd
SELECT * FROM comments c1 LEFT JOIN comments c2 ON c2.parent_id = c1.comment_id;
```
![](https://i.imgur.com/9GXOO7S.png)

2. Query a node's parent
```cmd
SELECT * FROM comments c1 LEFT JOIN comments c2 ON c1.parent_id = c2.comment_id;
```
![](https://i.imgur.com/sYBwrOc.png)

### Can't Handle Deep Trees
```cmd
SELECT * FROM comments c1 
LEFT JOIN comments c2 ON c2.parent_id = c1.comment_id 
LEFT JOIN comments c3 ON c3.parent_id = c2.comment_id 
LEFT JOIN comments c4 ON c4.parent_id = c3.comment_id
...
```
