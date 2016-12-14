# PTM

PTM submission code spark+python
---------------------------------------
1.original assignment reference: https://github.com/PaytmLabs/MachineLearningChallenge





---------------------------------------
2.business understanding reference: I need to collect knowledge for better understanding in e-commerce return.

> - a. return issue on e-commerce article : http://www.business2community.com/infographics/e-commerce-product-return-statistics-trends-infographic-01505394#VEJdw24y19uAqv0m.97

> - b. return deep-dive article : https://www.shopify.com/enterprise/102947142-how-to-reduce-your-return-rate-predict-exactly-what-customers-want






---------------------------------------
3.current understanding on PTM challenge from either data or industry article :

> - a. roughly 65% of return caused by seller  (see 2-a article)

> - b. return rate has some relationship with price level ( see 2-b article : "Marketing Land says higher return rates are generally associated with categories that offer expensive, fit-sensitive, and highly visible items.")

> - c. online discussion regarding PAYTM business and customer service: https://www.quora.com/What-is-your-review-of-Paytm-company

Many of people in the discussion loop said they found differnt product in shippoing box compared to original image in web site.
This is very risky customer response to e-commerce business.

> - d. Seller scoring might be mainly focus on their order fullfillment instead of revenue generation.  Since large volume or large revenue is caused by product category or underlying demand, order fullfillment is one of main key indicator to judge their overall performance. And this one should be very well managed otherwise # of complaint customers will grow and discourage customers to buy products from PAYTM.

> - e. order fullfillment: despite some delay, customers receive what they expect without seller's cancellation or return.

> - f. Seller scoring can stratch to area of higher commission or lower cashback. However data said the commission rate or cashback rate is tied to category level rather than seller level. 
> - **( Weight of feature : category >> other features ) which means once category is determined, their commission and cashback conditions would be tied to categories.**




---------------------------------------
>4. Study design
- target variable: normalized return ratio = ( cancellation + return ) / orderID count, if normalized return ration > 1, it should be capped with 1.
- clarify relationship between 6 month transaction data and overall profit/return matrix

**: hereafter, NRR = Normalized Return Ratio.**

**feature investigation : all features come from 6 month transaction data **

> - a. verify : price vs. NRR (hypothesis: more expensive-> seller might have issue to source them correctly )

> - b. verify : qty vs. NRR (hypothesis: more qty-> seller might have sourcing issue)

> - c. verify : shipping delay vs. NRR (hypothesis: more frequently shipping delay ->  higher issue on shipping overall)

> - d. verify : given SLA vs. NRR (hypothesis: if given SLN is longer-> return might be more than average)

> - e. verify : order hour, order day of week vs. NRR (hypothesis: if customers orders are piled in specific time or day -> it might cause seller's sourcing issue)

> - f. verify : shipping fulfillment vs. NRR ( hypothesis: longer from fulfillment creation to fulfillment end -> potential sourcing issue)

**data prep :** 
> - 1. dump PAYTM data (TX, profit/return matrix) to Databrick cluster
> - 2. set up Spark environment to run over Spark cluster with parallel processing
> - 3. build Spark SQL to get rank for each feature
> - 4. build pipeline for Spark ML (here, I used RandomForest classifier)
> - 5. Split dataset into **Training** and **Testing** sets
> - 6. Measure AUC for evaluation of model performance **(Binary prediction)**


**result **
> - AUC : ~ 0.65 (not fantastic)
> - scoring result : 0 to 1, decimal value


**Application : **
> - 1. run scoring for all sellers
> - 2. Sort them by scoring result
> - 3. low score group : investigate their underlying reasons of recurring order fulfillment issues
> - 4. high score group : providing more incentive to maintain their service level
