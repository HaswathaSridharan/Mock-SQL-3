# Mock-SQL-3
1 Problem 1 :  Market Analysis II	(https://leetcode.com/problems/market-analysis-ii/)
WITH CTE AS (
    SELECT seller_id, item_id, RANK() OVER(PARTITION BY seller_id ORDER BY order_date) AS rnk FROM Orders
), 

ACTE AS (
    SELECT CTE.seller_id, Items.item_brand FROM CTE JOIN Items ON CTE.item_id = Items.item_id WHERE rnk = 2
)

SELECT Users.user_id AS seller_id, IF(Users.favorite_brand = ACTE.item_brand, 'yes', 'no') AS 2nd_item_fav_brand FROM Users LEFT JOIN ACTE ON Users.user_id = ACTE.seller_id;

2 Problem 2: Tournament Winners(https://leetcode.com/problems/tournament-winners/description/)

SELECT group_id, player_id FROM (SELECT p.group_id, p.player_id, RANK() OVER(PARTITION BY p.group_id ORDER BY SUM(
    CASE
        WHEN p.player_id = m.first_player THEN m.first_score
        ELSE m.second_score
    END) DESC, p.player_id ASC) AS rnk FROM Players p JOIN Matches m ON p.player_id IN(m.first_player, m.second_player) GROUP BY p.group_id, p.player_id) AS intermediate 
WHERE rnk = 1;
                  