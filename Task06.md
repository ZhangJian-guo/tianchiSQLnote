本笔记为阿里云天池龙珠计划SQL训练营的学习内容，链接为：https://tianchi.aliyun.com/specials/promotion/aicampsql；

# 1 

    SELECT _is.TICKER_SYMBOL, _is.END_DATE, _is.T_REVENUE, _is.T_COGS, _is.N_INCOME,
            md.TICKER_SYMBOL, md.END_DATE, md.CLOSE_PRICE,
            co.TICKER_SYMBOL, co.INDIC_NAME_EN, co.END_DATE, co.VALUE
    FROM `income statement` _is, `market data` md, `company operating` co 
    WHERE _is.TICKER_SYMBOL = md.TICKER_SYMBOL and md.TICKER_SYMBOL = co.TICKER_SYMBOL
          and _is.TICKER_SYMBOL IS IN ('600383', '600048')
       
# 2

找出 pH=3.03的所有红葡萄酒，然后，对其 citric acid 进行中式排名（相同排名的下一个名次应该是下一个连续的整数值。换句话说，名次之间不应该有“间隔”）

    SELECT pH, citric acid, DENSE_RANK() OVER (ORDER BY citric acid) AS DENSE_RANK FROM `winequality-red` WHERE pH = 3.03
    
# 3

找出在2016年7月期间，发放优惠券总金额最多和发放优惠券张数最多的商家。

这里只考虑满减的金额，不考虑打几折的优惠券。
    
发放优惠券最多的商家：
    
    SELECT Merchant_id, COUNT(`User_id`) AS Discount_NUM 
    FROM `ccf_offline_stage1_test_revised` 
    WHERE Date_received BETWEEN '2016-07-01' AND '2016-07-31'
    GROUP BY Merchant_id
    ORDER BY Discount_NUM DESC
    LIMIT 1

发放优惠券金额最多的商家：

    SELECT Merchant_id, SUM(SUBSTRING_INDEX(`Discount_rate`,':',-1)) AS SUM_Discount
    FROM `ccf_offline_stage1_test_revised` 
    WHERE Date_received BETWEEN '2016-07-01' AND '2016-07-31'
    GROUP BY Merchant_id
    ORDER BY SUM_Discount DESC
    LIMIT 1
    
# 4


    
# 5

在2016年6月期间，线上总体优惠券弃用率为多少？并找出优惠券弃用率最高的商家。

总体弃用率：

    SELECT SUM(abandoned)/SUM(received)
    FROM
    (SELECT p1.Merchant_id AS Merchant_id, received, abandoned FROM 
    (SELECT Merchant_id, COUNT(User_id) AS received 
     FROM `ccf_online_stage1_train` 
     WHERE `Coupon_id` IS NOT NULL 
            AND Date_received BETWEEN '2016-06-01' AND '2016-06-30' 
     GROUP BY `Merchant_id`) 
     as p1
    LEFT OUTER JOIN  
    (SELECT Merchant_id, COUNT(User_id) AS abandoned 
     FROM `ccf_online_stage1_train` 
     WHERE `Coupon_id` IS NOT NULL AND `Date` IS NULL GROUP BY `Merchant_id`
            AND Date_received BETWEEN '2016-06-01' AND '2016-06-30')
     as p2
    ON p1.Merchant_id = p2.Merchant_id) p

CASE方法：

    SELECT SUM(CASE WHEN Date IS NULL AND Coupon_id IS NOT NULL
     THEN 1
     ELSE 0
     END) /
     SUM(CASE WHEN Coupon_id IS NOT NULL
     THEN 1
     ELSE 0
     END) AS discard_rate
    FROM ccf_online_stage1_train
    WHERE Date_received BETWEEN '2016-06-01' AND '2016-06-30';

弃⽤率最⾼的商家：


    SELECT Merchant_id, (abandoned/received) AS abandoned_rate
    FROM 
    (SELECT p1.Merchant_id AS Merchant_id, received, abandoned FROM 
    (SELECT Merchant_id, COUNT(User_id) AS received 
     FROM `ccf_online_stage1_train` 
     WHERE `Coupon_id` IS NOT NULL 
     AND Date_received BETWEEN '2016-06-01' AND '2016-06-30' 
     GROUP BY `Merchant_id`
    ) as p1
    LEFT OUTER JOIN  
    (SELECT Merchant_id, COUNT(User_id) AS abandoned 
     FROM `ccf_online_stage1_train` 
     WHERE `Coupon_id` IS NOT NULL AND `Date` IS NULL 
     AND Date_received BETWEEN '2016-06-01' AND '2016-06-30'
     GROUP BY `Merchant_id`)as p2
    ON p1.Merchant_id = p2.Merchant_id) p
    ORDER BY abandoned_rate DESC

CASE方法：

    SELECT Merchant_id, 
     SUM(CASE WHEN Date IS NULL AND Coupon_id IS NOT NULL 
     THEN 1
     ELSE 0
     END) /
     SUM(CASE WHEN Coupon_id IS NOT NULL
     THEN 1
     ELSE 0
     END) AS discard_rate
     FROM ccf_online_stage1_train
    WHERE Date_received BETWEEN '2016-06-01' AND '2016-06-30'
    GROUP BY Merchant_id
    ORDER BY discard_rate DESC
    LIMIT 1;


# 6

找出 pH=3.63的所有⽩葡萄酒，然后对其 residual sugar 量进⾏英式排名（⾮连续的排名）。

    SELECT  pH,  `residual sugar`, RANK() OVER (ORDER BY  residual sugar ) AS RANK FROM `winequality-white` WHERE `pH` = 3.63
    
# 7 

找出截止到2018年底市值最大的3个行业和每个行业市值最大的3个公司。

市值最大的3个行业：

    SELECT TYPE_NAME_CN,
     SUM(MARKET_VALUE)
     FROM `market data`
    WHERE YEAR(END_DATE) = '2018-12-31'
    GROUP BY TYPE_NAME_CN
    ORDER BY SUM(MARKET_VALUE) DESC
    LIMIT 3
    
每个行业市值最大的3个公司：

    
    
# 8

线上线下累计优惠券使用次数最多的顾客。

    SELECT p.User_id, COUNT(p.Coupon_id) AS times FROM 
    (SELECT * FROM `ccf_offline_stage1_train` WHERE `Coupon_id` IS NOT NULL AND `Date` IS NOT NULL AND LEFT(DATE,4) = 2016
    UNION
    SELECT * FROM `ccf_online_stage1_train` WHERE `Coupon_id` IS NOT NULL AND `Date` IS NOT NULL AND LEFT(DATE,4) = 2016) p
    GROUP BY p.User_id
    ORDER BY times DESC
    LIMIT 1
    
# 9



# 10


