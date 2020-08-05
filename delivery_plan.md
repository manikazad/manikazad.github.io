# Delivery Plan Optimization (Vehicle Routing Problem) :

### Objective : 
Reduce the delivery cost while improving the customer experience in terms of quicker deliveries within the SLA. 

Let me first explain the objective in simple words which will help you understand the current problems which this application helped to solve. Then we will dive into the more finer technical details. 

Let us set a background of the problem. 

The prime objective of the business is to deliver ordered items at your doorstep within 1 hour of ordering. The items are stored in warehouses from where each order is packed, checked out and dispatched with a delivery boy travelling on bikes and delivering in area of 5km - 10km radius around the warehouse. The challenge here is to deliver each order in time, with right medicines as fast as possible. All of this was to be achieved while saving on delivery cost. The business cares a lot about good customer experience and it was non-negotiable and we always focused on giving the best experience to each and every customer. I hope this gives enough peek into the problem to understand the rest of the post. 

Let us dive into more technical details starting with defining the problem. 

### Problem: 

On a very basic level our problem can be described a Travelling salesman problem for which multiple solutions have been proposed reaching near optimality. Our problem is more general version of TSP and can be called as Vehicle Routing Problem. To understand it more clearly, let's start with an ideal state condition: 
1. There are n orders to be dispatched.
2. There are m delivery boys in the warehouse such that m >= n
3. Each order has t_i time remaining in SLA (1 hour) (assuming, time remaining for each order is enough for delivering it).

**Reduced problem:** We have to assign n orders to m delivery boys such that SLA for no order is breached. 

**Ideal solution:** If m >= n, and there is enough time left for each order to be delivered then we just assign each order to a delivery executive in a one to one mapping and all the orders are delivered in time. That's it, problem solved. 
Only if the life was so easy !!

Neither do we have such ideal conditions available neither is our problem so simple. Along with fulfilling minimum criteria, we have to strive for saving the overall time and trips on all the deliveries combined. And we face a number of other challenges which can be understood with the following scenarios: 

1.  At any given time there can be about more than 100-200 orders in the dispatch area. (HIGH DIMENSIONAL)
2. Orders keep arriving every minute in the dispatch area. Hence the plan needs to be evaluated everytime a new order arrives or every minute, in search of more optimal solution. (STOCHASTIC PROBLEM)
2. When each of these orders reach the dispatch area the time remaining for each order to be delivered may vary anywhere between 5 to 50 minutes. (TIME WINDOWED)
3. Each executve may take maximum four orders with them. (CAPACITATED)
4. Another constraint that we face is the availability of the delivery executives in the warehouse, who can pickup and deliver the orders. Also, there are high demand peak times and low demand times during the day which needs to be handled in terms of fulfillment and resource optimization. (RESOURCE PLANNING and OPTIMIZATION)
5. There are executives who are on their way back to the warehouse after delivering and may be considered to be available for next trip in some time. (SCHEDULING WITH UNCERTAIN RESOURCES)
6. Consider a situation during the non-peak hours, there is just few orders (orders less than available delivery executives) ready for dispatch and with enough time for them to be delivered. Then do we wait for other orders to come which we could take along or do we dispatch it already without waiting. And if we wait then how much time would be optimal. (PROBABILISTIC OPTIMIZATION)

All of these problems are not only related but also dependent on each other and one cannot be solved (achieve optimality) independently without affecting another. 

### Working towards a solution: 

Like all optimization problem, the way to solution starts with defining the objective function. Formulating the objective functions is a tough task as we have to ask tough business questions to come up with the right optimality criteria. One of the tough questions, which I had to ask the CEO was how much does customer experience matter with respect to the money spent. This is a very critical questions, as with all business you have to spend wisely and on the other hand you cannot let it affect the experience for your customers. The answer to this questions is decided in terms of a factor which can be explained as a ratio between each unit of customer experience -to- each unit of money spent. Answering this questions can help clarify and objectively identify a lot of eye opening KPIs for the company. Lets say this factor is X, then you are willing to forego 1 unit of customer experience for each X units of money to be spent. Which can be understood as if I have to spend more than X money extra to achieve ! unit of customer experience the I may forego that customer experience. Now, the customer experience is subject to opportunity cost & management's discretion and can be changed according to the company's business strategy. For our purpose we took the 'time to delivery' as our unit of customer experience. 

The initial solution was to just optimize for assigning orders to delivery executives such that no order gets delayed. The system runs everytime a new order is ready for dispatch or every minute (whichever is earlier), generating a new plan which assigns a number of order in sequence to delivery executives. If not enough number of executives are available at current moment, then a priority is decided between the clubbed order groups based on primary criteria of due time for delivery. Many other such edge cases are handled based on business needs and resource constrains. 

Later, few more factors were introduced to enforce the clubbing of the orders which could be controlled by varying cost of delivery executive in the objective function.  Furthermore, the delivery executive cost was made a stochastic variable which was a function of the time of the day and varied according to the peak hours or slow hours. 

Given that there is a lack of delivery executives available in the warehouse at any moment we may fail to generate a plan for delivery. This led to predicting the time taken by delivery executives in returning to the warehouse and using this in creating a schedule with uncertain resources. 

As the number of orders increased it became imperative to clustering the orders geometrically around the warehouse. So that we can create delivery plans for each cluster separately. With increasing number of orders the number of permutations increase exponentially and it becomes defficult to search for an optimal solution in reasonable time. Clustering helps in the breaking the problem to smaller chunk and solving each chunk separately. 

Later on we felt the need to handle the problem number 6 described above. This required training a machine learning model to predict if a new order is going to arrive given the time of the day and if it will get clubbed with any of the orders which are already ready for dispatch. 

Further need arrived to start creating the delivery plan for an order while it is not yet ready for dispatch and is still in the process of checkout. This part again adds more uncertaininty to the system which needs to be handled dynamically. 

### Some Architectural Details:

The system was first written in python and given the slowness of loops in python and necessity of looping for generating possible delivery plans (set partitions) we needed a faster language which support extremely fast loops. Julia helped us immensely, givving almost 10-15x speed boost over python. The delivery planning system interacted with the main system using Redis Queues with blocking listeners. The request was composed of inter arrival times between multiple delivery locations in the request and other details about the state of the system. 

### Conclusions: 

Solving a vehicle routing problme is not a trivial task, specially when the dimensionality of the problem is high as the number of possible plans you could create explodes exponentially with every new node added to the plan. Hence finding a globally optimal solutions for this problem is almost impossible and you have to use other infromations sources (such as nearby location) to break down the problem into smaller problems, wherein optimal solutions can be found. This is observed that with more relevant information available beforehand you can reduce the search space more optimally, rather than just breaking the problem using greedy approaches which almost always lead to sub-optimal solutions. Another challenge in such problems is not having a benchamrk to beat or any universal success criteria. You can not know before hand how better your system can do untill it has done it, so the only way is to keep pushing the limit to do better by some small amount. 




