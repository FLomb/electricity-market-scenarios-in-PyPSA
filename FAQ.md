# Q&A - Inspired from past editions of the course

## 1. Where can I find data specific to the national electricity system I want to model?

*Be aware that some of the proposed data sources below are limited to (or they are richer for) the case of European countries.*

### Where can I find power plant capacity data?

As explained in the guidelines for the modelling assignment, detailed data on the current capacity of power plants of different kinds is available from powerplantmatching (see **tutorial #0**). Another option is to download power plant capacity data from [Open Power System Data](https://data.open-power-system-data.org/national_generation_capacity/2020-10-01), but it's less up to date.

Keep in mind that neither source is always precise. For most countries, there are small inconsistencies due to how data are reported by national bodies in charge of energy statistics. For instance, most countries do not report data about rooftop photovoltaic (PV) plants, leading to a severely underestimated existing solar PV capacity. We advise you to always integrate the results with some country-specific source (e.g., the website of or an annual report by a national energy regulator or statistics body) and adjust as needed.

In the Netherlands, the information is available from CBS: for example, [this page](https://opendata.cbs.nl/#/CBS/en/dataset/37823eng/table?ts=1764326716784) allows you to filter by 'Installation type'. If you select the different installations one by one, you can clearly see their installed capacity in 2024 and their main fuel. Let's say, for instance, that you select 'Internal combustion engines'. You can see that their total installed capacity is 3300 MW, and that their fuel input is natural gas. If you select 'steam turbines' instead, you'll see that the fuel is mostly coal, so you can assume that this is the capacity of coal power plants. Likewise, you'll see that 'combined cycle gas turbines' (CCGT) are purely fueled by gas, and so are the 'gas turbines' (OCGT).

### Where can I find power plant techno-economic parameters?

For the techno-economic parameters (efficiency, costs, etc.), we don't expect you to find country-specific data. It is perfectly fine to use the data from the PyPSA's Technology Data (see **tutorial #0**), which mostly relies on the data from the [Danish Energy Agency technology catalogues](https://ens.dk/en/analyses-and-statistics/technology-catalogues), a European reference widely used by all countries for modelling studies.

### Where can I find hourly electricity demand profiles?

The demand to be met by the power system in any given hour is also available from various open-access sources. The easiest option is [Open Power System Data (OPSD)'s time series database](https://data.open-power-system-data.org/time_series/2020-10-06)'; look for `load_actual_entsoe` in the columns of the downloaded file. As for other data in OPSD's database, however, you will hardly find anything more recent than 2020.

Another option is to use [ENTSO-E's transparency platform](https://transparency.entsoe.eu/). After creating a free account, you'll be able to access the 'File library', where you can look for demand data at high resolution for every month almost up to the present date.

### Where can I find renewable generators' hourly capacity factors?

The hourly capacity factors for renewable energy generation technologies can be gathered from various sources for many weather years. A very user-friendly and reliable source is [renewables.ninja](https://www.renewables.ninja/#/country), which already provides country-aggregated capacity factors, for weather years in the range 1980-2024. 

Be aware that **capacity factors and actual generation of renewables are not the same thing!** If you get renewable generation timeseries from, e.g., the ENTSO-E transparency platform, those will already account for curtailed (i.e., 'wasted') generation that renewables could have provided but didn't because there was no demand, storage or transmission capacity for it. In the model, you want to use the full generation potential (given by the product of the installed capacity and the hourly capacity factor) and let the model optimise the dispatch (including the decision to curtail renewable energy)!

### Where can I find real-world data for model validation?

For generation data (e.g., the resulting share of generated electricity by power plant type) you can rely on several publicly available sources. For the resulting prices, there are perhaps fewer options. A good suggestion is again [ENTSO-E's transparency platform](https://transparency.entsoe.eu/), which offers real-world data per EU country on both the economic (prices) and technical (actual generation) performance of the system in various historical years.


## 2. What modelling choices can I make for the project?

### Can I model the same country as in the tutorials, or should I change it?

The assessment criteria are those reported in the modelling assignment full description on Brightspace. The degree of change in the model is not per se a criterion, but of course, as stated in the class, we expect that you do not simply re-use the example model as-is.

If you are implementing a more realistic power-plant mix (e.g., by disaggregating into more categories, or by distinguishing power plants by their 'age' and penalising older ones), your research questions and scenarios should allow you to come up with a quite interesting analysis, because you will have multiple power plants affected by, e.g., the CO2 tax in different ways, compared to the example. 

### How well should my model approximate real-world data to be considered 'validated'?
     
The model and the real-world data will never be exactly the same, but may be more or less close depending on how finely you have approximated the different power plants of the country under study. E.g., if you have only one CCGT technology to represent a variety of fossil generators, price dynamics will hardly be the same. What matters, however, is that the difference you see reflects the simplifications you made, and you can thus explain it. And, moreover, that the high-level dynamics (e.g., the order of magnitude of the price, the peak/valley times of the day) are plausible. To make up for the difficulty of comparing a single day for a model with several approximations, a good solution may be to complement that with a more aggregate validation of the prices, such as looking at average daily/weekly/monthly prices over the entire year of operation. 

Keep in mind also that prices are not the only means of validation; even before prices, you should assess to what extent the yearly generation from different power plants and the dispatch dynamics approximate real-world ones. If the dispatch and/or yearly generation have a substantial deviation from real-world data, prices will, too.

## 3. How do I represent [something] in a PyPSA dispatch model?

For most code-related questions, you can find answers from [PyPSA's extensive documentation and gallery of examples](https://docs.pypsa.org/latest/). To make things even easier and quicker, you can also take advantage of **PyPSA Guru**. That is the AI model accessible from PyPSA's documentation (just click on 'Ask AI' from any page) and trained specifically on its contents. PyPSA Guru will provide more reliable results than generic AI models, avoiding possible mixing of old and new PyPSA examples.

See the example below:

**[You]**: *How can I represent long-duration power-to-hydrogen-to-power storage in an electricity dispatch model?*

**[PyPSA Guru]**:

Use a **hydrogen bus + electrolyser `Link` + hydrogen `Store` + turbine/fuel-cell `Link`**.

This is the pattern shown in the PyPSA examples for hydrogen storage systems.

1. Add a hydrogen `Bus`.
2. Add a charging `Link` from electricity to hydrogen, the electrolyser.
3. Add a `Store` on the hydrogen bus for long-duration energy storage.
4. Add a discharging `Link` from hydrogen back to electricity, such as a turbine or fuel cell.

Minimal pattern from the examples:

```python
n.add("Bus", "hydrogen", carrier="hydrogen")
```


```python
n.add(
    "Link",
    "electrolysis",
    bus0="electricity",
    bus1="hydrogen",
    carrier="electrolysis",
    p_nom_extendable=True,
    efficiency=costs.at["electrolysis", "efficiency"],
    capital_cost=costs.at["electrolysis", "capital_cost"],
)
```


```python
n.add(
    "Store",
    "hydrogen storage",
    bus="hydrogen",
    carrier="hydrogen storage",
    capital_cost=costs.at["hydrogen storage underground", "capital_cost"],
    e_nom_extendable=True,
    e_cyclic=True,
)
```


```python
n.add(
    "Link",
    "turbine",
    bus0="hydrogen",
    bus1="electricity",
    carrier="turbine",
    p_nom_extendable=True,
    efficiency=costs.at["OCGT", "efficiency"],
    capital_cost=costs.at["OCGT", "capital_cost"] / costs.at["OCGT", "efficiency"],
)
```

Why this is suitable for **long-duration** storage:
- `Store` provides inter-temporal storage on a single bus.
- `Store` energy capacity can be optimized independently with `e_nom_extendable=True`.
- charging and discharging power are controlled separately by the `Link` components, which is exactly the decoupled setup PyPSA recommends when power and energy capacities should be independent.

If you want a simpler but less flexible formulation with fixed power-to-energy ratio, PyPSA recommends `StorageUnit` instead, but the hydrogen examples use `Store` + `Link`, not `StorageUnit`.
