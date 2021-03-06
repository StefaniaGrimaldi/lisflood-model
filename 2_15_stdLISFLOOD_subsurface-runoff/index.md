## Sub-surface runoff routing

All water that flows out of the upper- and lower groundwater zone is routed to the nearest downstream channel pixel within one time step.
Recalling once more that the groundwater equations are valid for the pixel's permeable fraction only,the contribution of each pixel to the nearest channel is made up of 

$$
(f_{forest}+f_{other}+f_{irrigated}) \cdot (Q_{uz} + Q_{lz}). 
$$

The Figure below illustrates the routing procedure: for each pixel that contains a river channel, its contributing pixels are defined by the drainage network. 
For every 'river pixel' the groundwater outflow that is generated by its upstream pixels is simply summed. For instance, there are two flow paths that are contributing to the second \'river pixel\' from the left in Figure below. Hence, the amount of water that is transported to this pixel equals the sum of the amounts of water produced by these flowpaths, q1 + q2. 

Note that, as with the surface runoff routing, no water is routed *through* the river network at this stage.

![Routing of groundwater to channel network](../media/image35.png)
***Figure:*** *Routing of groundwater to channel network. Groundwater flow is routed to the nearest 'channel' pixel.*

[🔝](#top)
