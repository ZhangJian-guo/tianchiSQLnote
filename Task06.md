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


    
