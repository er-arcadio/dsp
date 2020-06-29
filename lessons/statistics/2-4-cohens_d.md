[Think Stats Chapter 2 Exercise 4](http://greenteapress.com/thinkstats2/html/thinkstats2003.html#toc24) (Cohen's d)

# Exercise 2.4

We will do several things in this section...
1. We will compare 1st babies' weights with other babies' weights on a graph
2. We will calculate Chohen's Effect Size for the babies' weights
3. We will compare 1st babies' and other babies' preg. length & calculate Cohen's d
4. We will compare the 2 effect sizes.

### Seperating and Comparing Weights

Here is the code to import the data, filter it so that there is only "live birth" outcomes showing, and separating the data into 2 groups: 1st babies and other babies.
```python
import nsfg

preg = nsfg.ReadFemPreg()   #reads in the data
live = preg[preg.outcome==1]   #filters the data - only keeping live births no misscariages etc.

# Separating the first births with the other births
firsts = live[live.birthord==1]
others = live[live.birthord != 1]
```

Here is the code for putting the two groups' distributions on the same graph and the output of such.
```python
import thinkstats2 as ts2
import thinkplot

# Creating 2 dictionaries for the two categories (1st babies and other babies)
first_wgt_hist = ts2.Hist(firsts.birthwgt_lb, label='first')
other_wgt_hist = ts2.Hist(others.birthwgt_lb, label='other')

# Graphing them on the same graph
width = 0.45
thinkplot.PrePlot(2)
thinkplot.Hist(first_wgt_hist, align='right', width=width)
thinkplot.Hist(other_wgt_hist, align='left', width=width)
thinkplot.Show(xlabel='weight', ylabel='frequency')
```


![png](https://github.com/er-arcadio/My_metis_pngs_2020/blob/master/output_3_0.png)



    <Figure size 576x432 with 0 Axes>


We have a visual representation, but it would be more telling if we used "Cohen's d" to compare the two means and get a measurement on their difference.

### Cohen's Effect Size of Babies' Weights


Here is the code for a method that computes Cohen's Effect Size.
```python
import math

def CohenEffectSize(group1, group2):
    diff = group1.mean() - group2.mean()   # The numerator of the "Cohen's d" formula
    
    # the "pooled variance" - s^2 - denominator of the formula would be s
    var1 = group1.var()
    var2 = group2.var()
    n1, n2 = len(group1), len(group2)
    pooled_var = (n1 * var1 + n2 * var2) / (n1 + n2)
    
    #putting it all together
    d = diff / math.sqrt(pooled_var)
    
    return d
```

Calculating Cohen's d for the 2 groups' birthweight.
```python
d_weight = CohenEffectSize(firsts.birthwgt_lb, others.birthwgt_lb)
d_weight, d_weight*16   # displaying the result in pounds and ounces.
```




    (-0.10845024254407831, -1.735203880705253)



This means that the mean weight of the 1st babies are lighter than that of the other babies. However, 0.1 lbs or 1.7 oz isn't significant enough to make any claim.

Now we look at the length of pregnancy for each of the 2 groups.

### Comparing Pregnancy Length

Here is the code to graph the length of pregnancy for the 1st baby and the length for the other babies on the same graph
```python
# Now these dictionary-like objects will be based off the prglngth
first_ln_hist = ts2.Hist(firsts.prglngth, label='first')
other_ln_hist = ts2.Hist(others.prglngth, label='other')

# Graphing them on the same graph
width = 0.45
thinkplot.PrePlot(2)
thinkplot.Hist(first_ln_hist, align='right', width=width)
thinkplot.Hist(other_ln_hist, align='left', width=width)

# Here we limit the width to 27 - 46 weeks ensuring that the data doesnt contain any outliers or errors
thinkplot.Show(xlabel='weeks', ylabel='frequency', xlim=[27, 46])  
```


![png](https://github.com/er-arcadio/My_metis_pngs_2020/blob/master/output_8_0.png)



    <Figure size 576x432 with 0 Axes>


Again, the graph doesn't give a good representation of the desired comparison, so we use Cohen's d to get a measurement of the difference (if there is any).



```python
d_length = CohenEffectSize(firsts.prglngth, others.prglngth)
d_length, d_length*7*24   # displaying the length in weeks and hours
```




    (0.028879044654449883, 4.85167950194758)



Here we see an even less significant difference of about .03 weeks or about 5 hours later rather than earlier. 


### Conclusion

If we want to say something about the female's 1st baby in comparison to their other babies, we can say that their 1st baby tends to weigh less (by about 2 ounces) and arrive later (by about 5 hours). However, this is only a sample, and, because of the small effect sizes, it should not be taken seriously but rather be further looked into with perhaps a resample at a bigger sample size. 


