SELECT date(time) as date,
       sum(order_price) as revenue FROM(SELECT order_id,
                                        sum(price) as order_price
                                 FROM   (SELECT order_id,
                                                product_id,
                                                price FROM(SELECT order_id,
                                                           unnest(product_ids) as product_id
                                                    FROM   orders) as t1
                                             LEFT JOIN products using (product_id)) as t2
                                 GROUP BY order_id) t3
    LEFT JOIN user_actions using (order_id)
WHERE  order_id not in (SELECT order_id
                        FROM   user_actions
                        WHERE  action = 'cancel_order')
GROUP BY date
ORDER BY date
