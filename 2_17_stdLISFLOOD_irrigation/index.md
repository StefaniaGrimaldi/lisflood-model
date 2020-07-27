## Irrigation


### Crop irrigation

Crop irrigation and Paddy-rice irrigation are dealt with by seperate model subroutines and are described in different chapters.
They can be switched on by adding the following lines to the 'lfoptions' element:

```xml
<setoption choice="1" name="drainedIrrigation"/>
```


### Paddy-rice irrigation

Paddy-rice irrigation is modelled using a dedicated routine which can be switched on by adding the following lines to the 'lfoptions' element:

```xml
<setoption choice="1" name="riceIrrigation"/>
```
Water demand, use, and return are computed according to three main phases: planting, growing, and harvesting. For this purpose, planting day and harvest day are required as input data.
* Field preparation before planting: it is assumed that the soil is saturated in 10 days, starting from 20 days before the planting day. The rice water demand during each day of this phase is then computed as

$$ 
Rice_demand_planting = 0.1 \cdot [(WS1-W1)+(WS2-W2)] \cdot RiceFracton \cdot \Delta t 
$$

where $WS1 is the saturation

![](https://latex.codecogs.com/gif.latex?%5Cfrac%7Ba%7D%7Bb%7D)
