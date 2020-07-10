# Demand Forecasting and Inventory Optimization 

### Problem: 
The company was reeling under high capital expenditure due to high levels of inventory cost. Further the fulfillment percent was quite lesser than the desired levels. So the problem was to increase the fulfillment percentages while reducing the inventory cost and keeping dead stock as low as possible.

### Solution: 
Optimization of Inventory to maximize the fulfillment and minimizing the cost. A data products for forecasting demand and optimizations the inventory was developed which helped attain the business goals and helping the business survive. 
* Developed a near real life simulation environment to experiment with models and achieve a realistic view of the model performance.
* Implemented the solution which gives weekly forecasts in production and responsible for managing it as well.
* Developed a monitoring tool which tracks the expected & actual performance and sends daily report to the stakeholders.

### Benefits: 
* The model attained an increment in item fulfillment to 90% from earlier figure of only 65%
* Brought down the inventory cost from 60 days to 30 days, resulting in reduced working capital expenditure and increased ROI


## Solving the Problem, Explained: 

1. The first few iterations involved trying out models such as linear regression models such as GLM, trained separately for each SKU. This didn’t work out well. 
2. Next, More complex, non-linear models such as XGB, SVR, NN were also tried to achieve better accuracy in demand prediction but all efforts to no avail. 
3. It became clear that before building the model and improving the predictions, first we need to set the right evaluation criteria for the prediction and it should be according to the business needs and not according to any general predictive model evaluation criteria. 
4. It was hence decided that the item fulfillment in a week is the right criteria which was agreed upon by the CXOs of the company.
5. To calculate this criteria, there was need of simulation because each and every order matters and adds up-to the total fulfillment. So even though the quantity bought might be more than what was demanded, but if it is not available at right time then it might not cater to the demand. And hence fulfillment might not be good even though enough quantity is ordered.
6. A simulation code, which simulates the whole supply chain process of the company was developed. There were parameters such as multiple suppliers, delay in receiving orders for each supplier, delay in inventory input and more such parameters which were critical to the inventory management and ordering process. 
7. Then more investigation was done on demand data at SKU level, trying to understand and observe patterns if any. Techniques such as clustering, rule based categorization, and other such methods were applied to gain insights into the SKU demand behavior. Factors such as ordering quantity, ordering speed, ordering frequency, the period of ordering to be considered, association between drugs etc were investigated too. 
8. On deeper investigation into the demand data, some patterns based on  ordering frequency was observed among the SKUs. Based on it the SKUs could be broadly divided into 3 categories given as Fast Moving, Slow Moving and Very Slow Moving. And on finer classification, these categories were further divided into total 7 classes. 
9. Now it was decided that we should target each category differently if needed and try to increase fulfillment in each category separately. 
10. Due to urgency of the situation and lack of time a quick solution was desired 
11. Hence investigation went into such direction which would yield a quick solution and a rule based prediction model is decided upon. 
12. For a rule based model we needed to create least amount of rules, which could generate forecast for all the medicines. Hence a simple rule based demand prediction model was created based on previously defined 7 classes. 
13. This model would generate the amount of all medicines which need to stocked up for upcoming week. (1 week in advance.) So that during the current week the ordering of all the required medicine can be planned. All of these factors were also taken into account in the simulation as well. 
14. For achieving an optimum performance with this rule based model, hundred of rules were tried upon with a report generated for each strategy, which is used to make further improvements and reach upon next rule, which is again tried upon. 
15. The achieved rules were tried against multiple real life scenarios in the order simulation and report is generated for each. 
16. With this manual and tedious but quick process, we were finally able to achieve above 87% item fulfillment and above 80% order fulfillment on multiple week on week basis. 
17. The simulation is run for multiple weeks (8-10) data and then the performance is measured. And then the strategy is decided upon
18. Further care was taken in the rules to avoid a build up of dead-stock(which doesn’t get sold). 


### Algorithms Used: 
	Divide (Categorize) and Rule Based

### Special / Specific Insights: 

Business Success Criteria is more important than Model Evaluation Criteria. Hence always optimize for business success not just model’s performance in isolation, which might not make any sense to business.

### Learnings: 
1. A dirty solution in nick of time is much better than a perfect solution arrived at too late. 
2. At the end of the day it should work, it doesn’t matter much that which method is technology has been used at the back. 