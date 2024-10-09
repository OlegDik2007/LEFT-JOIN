SELECT order_id
FROM   (SELECT order_id,
               time as delivery_time
        FROM   courier_actions
        WHERE  action = 'deliver_order') as w
    LEFT JOIN orders using(order_id)
ORDER BY delivery_time - creation_time desc limit 10;
