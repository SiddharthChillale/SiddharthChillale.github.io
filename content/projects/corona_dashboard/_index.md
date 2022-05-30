---
title: Corona Dashboard devlog
author: Siddharth
date: 2020-04-10
summary: My collection of things I find on my journeys through the internet.
categories: [Graphics]
tags: [coding, data science, projects]
ShowToc: true
cover:
  image: "images/new_to_total[updated].png"
  alt: "dashboard[updated-1].png"
  caption: "A chart showing covid trends"
  relative: true
---

## Version 2.0.1 - Date : April 16, 2020

__Final__

{{< figure src="images/new_to_total[updated].png" caption="Example of the created GitHub repo and url to clone it." >}}
- The numbers had very extreme values due to inconsistency of data. Since we are focusing only on the trend, we do not require exact numbers. Therefore, a rolling mean of 5 values is implemented to smoothen out the irregularities.

__Initial__

{{< figure src="images/new_to_total[old].png" caption="Example of the created GitHub repo and url to clone it." >}}
- Added a graph plotting new cases to existing cases.
- The Graph is number of New cases per Total number of cases. In order to understand exponential growths for diseases, we must not plot the growth against time (which is not so useful in prediction) ; but with the total existing numbers.[^1]
- A fall in the graph indicates lowering of new cases with total cases. i.e. We see that China and Korea are out of the danger of new cases. Italy and Spain are seeing small rates of controling the virus.
- Axes are plotted on logarithmic scale.


---
## Version 2.0   Date: March 23-27, 2020


{{< figure src="images/dashboard[updated-1].png" caption="Example of the created GitHub repo and url to clone it." >}}

{{< figure src="images/dashboard[updated-2].png" caption="Example of the created GitHub repo and url to clone it." >}}

__Updates__

- Added a Death Count Graph.
- Changed the theme of the dashboard to look more presentable.
- Added functionality of updating data in every refresh or at intervals of 6 hours.



---
## Version 1.0   Date: March 12-18, 2020

{{< figure src="images/dashboard[old-1].png" caption="Example of the created GitHub repo and url to clone it." >}}
{{< figure src="images/dashboard[old-2].png" caption="Example of the created GitHub repo and url to clone it." >}}

- Implemented a Confirmed Cases count graph.
- Implemented a New cases per day graph.
- Implemented a Pie chart for comparing proportions.
- Implemented a Table to give info on daily new cases.
- Deployed in production mode to Heroku.



[^1]: [How To Tell If We're Beating COVID-19 - minutephysics](https://youtu.be/54XLXg4fYsc?t=169)
