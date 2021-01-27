## Closure Table
### Intro
1. Many-to-many table. 
2. Stores every path from each node to each of its descendants.
3. A node even connects to itself. 

Illustration  
![](https://i.imgur.com/t85xZ8z.png)

```cmd
CREATE TABLE tree_paths (
	ancestor INT NOT NULL,
	descendant INT NOT NULL
);

INSERT INTO `tree_paths` (`ancestor`, `descendant`)
VALUES (1,1), (1,2), (1,3), (1,4), (1,5), (1,6), (1,7),
       (2,2), (2,3),
       (3,3),
       (4,4), (4,5), (4,6), (4,7),
       (5,5),
       (6,6), (6,7),
       (7,7);
```

![](https://i.imgur.com/Ebrp5C7.png)  

### Query Descendants of #4
1. Join comments and tree_paths  
```cmd
select * FROM comments c JOIN tree_paths t 
ON (c.comment_id = t.descendant)
```

![](https://i.imgur.com/C0mTraO.png)

2. Query Descendants of #4  
```cmd
select c.* FROM comments c JOIN tree_paths t 
ON (c.comment_id = t.descendant)
WHERE t.ancestor = 4;
```
![](https://i.imgur.com/8i1oy6I.png)
![](https://i.imgur.com/7xBYXpV.png)

### Query Ancestors of #6
1. Join comments and tree_paths  
```cmd
select * FROM comments c JOIN tree_paths t 
ON (c.comment_id = t.ancestor)
```

![](https://i.imgur.com/bxKufGD.png)

2. Query Ancestors of #6  
```cmd
select c.* FROM comments c JOIN tree_paths t 
ON (c.comment_id = t.ancestor)
WHERE t.descendant = 6;
```
![](https://i.imgur.com/5jg9LID.png)
![](https://i.imgur.com/1qoFqqH.png)

### Insert New Child of #5
```cmd
INSERT INTO comments (comment_id, author, comment) VALUES (8, 'Fran', 'I agree!');
INSERT INTO tree_paths (ancestor, descendant) 
SELECT ancestor, 8 FROM tree_paths 
WHERE descendant = 5 
UNION ALL SELECT 8,8;
```
![](https://i.imgur.com/6xgCOqH.png)

### Delete Child #7
```cmd
DELETE FROM tree_paths WHERE descendant = 7
```
![](https://i.imgur.com/hegxzAz.png)

### Delete Subtree under #4
```cmd
DELETE FROM tree_paths 
WHERE descendant IN 
(SELECT descendant FROM tree_paths WHERE ancestor = 4)
```
![](https://i.imgur.com/xJdhBeX.png)
![](https://i.imgur.com/Mdr70Hb.png)

### Path Length
1. Add a length column.
2. Max(length) is the depth of tree.
3. Make it easier to query immediate parent or child.  
![](https://i.imgur.com/hCVmaM8.png)
```cmd
select c.* FROM comments c JOIN tree_paths t 
ON (c.comment_id = t.descendant)
WHERE t.ancestor = 4
AND t.length = 1;
```
