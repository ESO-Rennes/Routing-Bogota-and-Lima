This work was carried out by [Hugo Thomas](https://perso.univ-rennes2.fr/hugo.thomas) and [Florent Demoraes](https://perso.univ-rennes2.fr/florent.demoraes) at the CNRS ESO research unit in Rennes (France), between June 2023 and December 2025. This study is part of a PhD thesis developed by Hugo Thomas (Université Rennes 2, France / Universidad de los Andes, Colombia) and also part of the activities of the Modural program funded by the French National Research Agency. The methods developed in R and applied to Bogotá and Lima are replicable to other urban areas and other mobility surveys using other travel modes.

This repository contains scripts to carry out network-based distance computation based on a Household Travel Survey conducted in 2023 in Bogotá, and a Household Travel Survey conducted in 2023 in Lima. The analysis uses databases representing approx. 23,000 households in Bogotá and 6,000 households in Lima, 100,000 trips in Bogotá and 32,000 trips in Lima, as well as road and public transport networks downloaded from OpenStreetMap or open data repositories. 

The main outputs of this section are:

* The trips databases of both cities with additional variables accounting for the distance, greenhouse gas (GHG) and air pollutant emissions of each trip.
* Maps of GHG emissions per zone of residence.
* Bivariate choropleth maps of mode-use intensities: number of trips per 1,000 inhabitants and distance per trip.
* Tables with an indicator for equity anaylisis (Palma ratio).

# Trips origin and destination

For each sampled trip, we know its origin and destination, the main mode used, and its duration, among other variables of interest. Each trip is thus described as an origin-destination pair. For this study, and under a confidentiality agreement, we obtained from the SDM and ATU the precise locations of the trip origins and destinations.

# Mode choice, routing and distance computation

Each trip in both surveys is assigned a main mode of travel according to a modal hierarchy. Thus, we do not need to estimate mode assignment models. To infer itineraries between origins and destinations, we then processed routing with different methods depending on the mode and city: For Bogotá’s Integrated Public Transport System (SITP) only, we computed [routing through the bus network](Distancias_SITP_r5r.Rmd), looking for the fastest path. Indeed, the availability of General Transit Feed Specifications (GTFS) files allows to take timatables into account. To do so, we used the R package [r5r](https://ipeagit.github.io/r5r/). This package supports one-to-one intermodal routing (walk, car, public transport) and has already been used in Conway et al. (2018) work for accessibility analysis, among others. 

Due to the absence of existing route assignement model nor public transport GTFS in Lima, we computed the shortest path in the metro, BRT and bus networks provided by the Lima and Callao Urban Transport Authority (ATU). We used [sfnetworks]( https://luukvdmeer.github.io/sfnetworks/), a powerful R package designed to quickly compute one-to-one routing. It is also quick enough to compute a many-to-many distance matrix. It is particularly accurate for routing in single-weighted graphs, such as public transport routes, and is compatible with sf objects .
Finally, in both cities, we pooled the remainder of modes in three categories (walking, bicycle, all other motorized such as car and motorcycle) and computed the shortest paths through dual-weighted road networks, applying a distinct edge hierarchy depending of the mode, priorizing trunk roads for motorized vehicle and bicycle lanes and local roads for bikes. A third R package, [dodgr](https://urbananalyst.github.io/dodgr/articles/dodgr.html), was preferred to compute distances on dual-weighted graphs, that are better suited to describe hierarchized networks with several highway categories.

Please find attached the R scripts for [Bogotá](Distancias_EODH_2023_projection_reseau.Rmd) and [Lima](Distancias_ATU_2023_con_caminata_previa.Rmd).

# Emissions calculation

Emissions were first computed for each trip from the samples. Following the GHG Protocol for mobile sources, emission factors for GHG include direct engine emissions (Scope 1) and those resulting from fuel and/or electricity production (Scope 2). GHG taken into account in this study include carbon dioxide (CO2), methane (CH4) and nitrous oxide (N2O).  

Direct engine emission factors (Scope 1) use country-specific values for Colombia and Peru. Emission factors resulting from the production of fossil fuels (Scope 2) are generic emission factors recommended by the IPCC guidelines. Emission factors resulting from the electricity production use a specific value for the Colombian electricity mix. 

Vehicle fuel consumption and load factors are place-specific: In Bogotá, they were established by the Colombian Mines and Energy Planning Unit (UPME); however, values are rather ancient (before 2015) and do not take into account massive fleet redeployment, specifically for public transport. In Lima, distinct values were established by the Ministry of Environment for taxis, regular buses and all other vehicles. For a given vehicle category, when different subcategories exist (e.g. different versions of TransMilenio buses with distinct fuels), we calculated an average emission factor weighted by the number of vehicles in each subcategory. We were provided with vehicle inventories by the Bogotá Environment Department (SDA) and Peru Environment Ministry (MINAM).

# Sampled Maps

![ges_bgta.png](https://github.com/ESO-Rennes/Routing-Bogota-and-Lima/blob/main/ges_bgta.png)

![ges_lima.png](https://github.com/ESO-Rennes/Routing-Bogota-and-Lima/blob/main/ges_lima.png)

![typo_auto_lima_1.png](https://github.com/ESO-Rennes/Routing-Bogota-and-Lima/blob/main/typo_auto_lima_1.png)

![typo_car_bogota.png](https://github.com/ESO-Rennes/Routing-Bogota-and-Lima/blob/main/typo_car_bogota.png))

