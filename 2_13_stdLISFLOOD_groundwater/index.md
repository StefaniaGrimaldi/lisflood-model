## Groundwater

Groundwater storage and transport are modelled using two parallel linear reservoirs, similar to the approach used in the HBV-96 model (Lindström et al., 1997). The upper zone represents a quick runoff component, which includes fast groundwater and subsurface flow through macro-pores in the soil. The lower zone represents the slow groundwater component that generates the base flow. The **outflow from the upper zone to the channel**, $Q_{uz} [mm]$ equals:

$$
Q_{uz} = \frac{1}{T_{uz}} \cdot UZ \cdot \Delta t
$$

where $T_{uz}$ is the reservoir constant for the upper groundwater layer $[days]$ and $UZ$ is the amount of water that is stored in the upper zone $[mm]$. 

The amount of water stored in the upper zone $UZ$ is computed as follows:

$$
UZ = $D'{2,gw}$ + D_{pref,gw} - D_{uz,lz}
$$

where $D'{2,gw}$ is the flux from the lower soil layer to groundwater for this sub-step; $D_{pref,gw}$ is the amount of preferential flow per time step; D_{uz,lz} is the amount of **water that percolates from the upper to the lower zone**, all in $[mm]$.

In areas with drained irrigation ($DrainedFraction$), the flux from the lower soil layer to groundwater $D'{2,gw}$ is directly delivered to the river channel, consequenlty, the computation of $UZ$ is modifies as follows:

$$
UZ = (1 - DrainedFraction) \cdot $D'{2,gw}$ + D_{pref,gw} - D_{uz,lz}
$$

In areas with flooded irrigation (e.g. rice crops), an additional amount of water is added to $UZ$ during the percolation and draining phases of the agricultural cycle ($RicePercolationeWater$ and $RiceDrainageWater$ are described in **ADD LINK TO CHAPTER**).

The **water percolates from the upper to the lower zone** ($D_{uz,lz}$) is the inflow to the lower groundwater zone. As indicated above, this amount of water is provided by the upper groundwater zone.  $D_{uz,lz}$ is a fixed amount per computational time step and it is defined as follows:

$$
D_{uz,lz} = min (GW_{perc} \cdot \Delta t ,UZ)
$$

Here, $GW_{perc} \ [\frac{mm}{day}]$ is the maximum percolation rate from the upper to the lower groundwater zone. going from the Upper to the Lower
                    
The **outflow from the lower zone to the channel** is then computed by:

$$
Q_{lz} = \frac{1}{T_{lz}} \cdot LZ \cdot \Delta t
$$

Here, $T_{lz}$ is the reservoir constant for the lower groundwater layer ($[days]$), and $LZ$ is the amount of water that is stored in the lower zone ($[mm]$). 

$LZ$ is computed as follows:

$$
LZ = D_{uz,lz}  - totGW_{abstr} - ( GW_{loss} \cdot \Delta t ) 
$$

where $D_{uz,lz}$ is the percolation from the upper groundwater zone ($[mm]$); totGW_{abstr} is the total amount of **water abstracted from groundwater** to comply with domestic,industrial, irrigation, and livestock demand ($[mm]$);$GW_{loss}$ is the maximum percolation rate from the lower groundwater zone ($[\frac{mm}{day}]$). 
The amount of water defined by $GW_{loss}$ never rejoins the river channel and it's lost beyond the catchment boundaries or to deep groundwater systems. $GW_{loss}$ is set to zero in catchments were no information is available. The larger the value of $GW_{loss}$, the larger the amount of water that leaves the system.

The values of both $T_{uz}$ ($[days]$), $T_{lz}$ ($[days]$), $GW_{perc}$ ($[\frac{mm}{day}]$), and $GW_{loss}$ ($[\frac{mm}{day}]$) are obtained by calibration. 
To avoid spurious results (**and mass balance errors - BE SURE OF THIS**), when $GW_{perc}$<$GW_{loss}$, $GW_{perc}$ is set equal to $GW_{loss}$.

Note that these equations are valid for the permeable fraction of the pixel only: storage in the direct runoff fraction equals 0 for both $UZ$ and $LZ$.

 M


## Groundwater abstractions

LISFLOOD includes the option of groundwater abstraction for irrigation and other usage purposes (see water use chapter). LISFLOOD checks if the amount of demanded water that is supposed to be abstracted from a source, is actually available. 

Groundwater abstraction = the total water demand * fracgwused

In the current LISFLOOD version, groundwater is abstracted for a 100%, so no addtional losses are accounted for, by which more would need to be abstracted to meet the demand. Also, in the current LISFLOOD version, no limits are set for groundwater abstraction.

LISFLOOD subtracts groundwater from the Lower Zone (LZ). Groundwater depletion can thus be examined by monitoring the LZ levels between the start and the end of a simulation. Given the intra- and inter-annual fluctuations of LZ, it is advisable to monitor more on decadal periods.

If the Lower Zone groundwater amount decreases below the 'LZThreshold" - a groundwater threshold value -, the baseflow from the LZ to the nearby rivers is zero. When sufficient recharge is added again to raise the LZ levels above the threshold, baseflow will start again. This mimicks the behaviour of some river basins in very dry episodes, where aquifers temporarily lose their connection to major rivers and baseflow is reduced.

```xml
<textvar name="LZThreshold" value="$(PathMaps)/lzthreshold.map">
<comment>
threshold value below which there is no outflow to the channel
</comment>
</textvar>
```

These threshold values have to be found through trial and error and/or calibration. The values are likely different for various (sub)river basins. You could start with zero values and then experiment, while monitoring simulated and observed baseflows. Keeping large negative values makes sure that there is always baseflow.

When groundwater is abstracted for usage, it typically could cause a local dip in the LZ values (~ water table) compared to neigbouring pixels. Therefore, a simple option to mimick groundwaterflow is added to LISFLOOD, which evens out the groundwaterlevels with neighbouring pixels. This option can be switched on using:

```xml
	<setoption choice="1" name="groundwaterSmooth"/>
```


[🔝](#top)