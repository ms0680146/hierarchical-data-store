# hierarchical-data-store
Mysql處理樹形結構的資料常見的方式有以下四種:  
- Adjacency List: 每一筆資料存parent_id
- Path Enumerations: 每一筆資料存整個tree path經過的node列舉
- Nested Sets: 每一條筆資料存nleft和nright
- Closure Table: 維護一個表，所有的tree path作為記錄進行儲存。

各方法優缺點比較表:  
![](https://i.imgur.com/ae4cWtu.png)