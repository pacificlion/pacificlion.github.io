---
published: true
layout: post
title: Comparison of Temperature deviations over year
---

![Comparison of Temperature deviations over year](temperatureDeviations.png "Comparison of Temperature deviations over year")
The horizontal axis represents the Year. The vertical axis shows temperature deviations. The  black color depicts global temperature, blue depicts temperature of Northern hemisphere and red for temperature of the Southern hemisphere. 

I used the dataset provided by [GISTEMP](http://data.giss.nasa.gov/gistemp/). Though I had to smoothen the curve to show continuity for which I used gaussian_filter1d and plot the data using matplotlib library in python.


The visualization clearly depicts the rise in average temperature with each year.

### Code is as follows

```python

# import libraries
from pandas import read_csv
from pandas import to_numeric
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from scipy.ndimage.filters import gaussian_filter1d

# import data
df = read_csv('ExcelFormattedGISTEMPData2CSV.csv')
df.sort_values(by=['Year'], ascending=True, inplace=True)
print(df.describe())



#plot graph

builds = df["Year"]
y_stack = np.row_stack((df["Glob"], df["NHem"], df["SHem"])) 
y_stack[0,:] = gaussian_filter1d(df["Glob"], sigma=0.8)
y_stack[1,:] = gaussian_filter1d(df["NHem"], sigma=0.8)
y_stack[2,:] = gaussian_filter1d(df["SHem"], sigma=0.8)

fig = plt.figure(figsize=(11,8))
ax1 = fig.add_subplot(111)
ax1.set_xlim(1880,2020)
ax1.set_ylim(-100,100)

ax1.plot(builds, y_stack[0,:], label='Global', color='#CDCDCD',linewidth='2')
ax1.plot(builds, y_stack[1,:], label='North Hemisphere', color='#9D9DFF',linewidth='2')
ax1.plot(builds, y_stack[2,:], label='South Hemisphere', color='#FFC5C5',linewidth='2')
leg=ax1.legend()
for line in leg.get_lines():
    line.set_linewidth(4)
plt.xlabel('Year')
plt.ylabel('Temperature Deviations')

# ax1.grid()

fig.savefig('Deviations.png', bbox_inches='tight')
plt.close(fig)    # close the figure window
```

