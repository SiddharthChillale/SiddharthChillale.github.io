---
title: Corona Dashboard devlog
author: Siddharth
date: 2020-04-10
---

### Version 2.0.1 - Date : April 16, 2020

__Final__

![sable](/Corona/new_to_total[updated].png)

- The numbers had very extreme values due to inconsistency of data. Since we are focusing only on the trend, we do not require exact numbers. Therefore, a rolling mean of 5 values is implemented to smoothen out the irregularities.

__Initial__

![](/Corona/new_to_total[old].png)

- Added a graph plotting new cases to existing cases.
- The Graph is number of New cases per Total number of cases. In order to understand exponential growths for diseases, we must not plot the growth against time (which is not so useful in prediction) ; but with the total existing numbers.[^1]
- A fall in the graph indicates lowering of new cases with total cases. i.e. We see that China and Korea are out of the danger of new cases. Italy and Spain are seeing small rates of controling the virus.
- Axes are plotted on logarithmic scale.


---
### Version 2.0   Date: March 23-27, 2020

![](/Corona/dashboard[updated-1].png)
![](/Corona/dashboard[updated-2].png)

__Updates__

- Added a Death Count Graph.
- Changed the theme of the dashboard to look more presentable.
- Added functionality of updating data in every refresh or at intervals of 6 hours.



---
### Version 1.0   Date: March 12-18, 2020

![](/Corona/dashboard[old-1].png)
![](/Corona/dashboard[old-2].png)

- Implemented a Confirmed Cases count graph.
- Implemented a New cases per day graph.
- Implemented a Pie chart for comparing proportions.
- Implemented a Table to give info on daily new cases.
- Deployed in production mode to Heroku.



[^1]: [How To Tell If We're Beating COVID-19 - minutephysics](https://youtu.be/54XLXg4fYsc?t=169)
