---
title: "Emissions from HPC"
teaching: 15
exercises: 0
questions:
- "How are emissions from HPC systems measured?"
- "How can I estimate the emissions from my use of HPC systems?"
- "How can I reduce the emissions generated from my use of HPC systems?"
objectives:
- "Describe the sources of emissions from HPC systems."
- "Describe how use of HPC systems can reduce emissions."
- "Show how emissions from an HPC system can be audited."
- "Describe how I can reduce the emissions from my use of HPC systems."
keypoints:
- "Main sources of emissions from HPC systems are from electricity use (scope 2) and embodied emissions in the hardware (scope 3)."
- "Research conducted on HPC systems contributes to reducing global emissions."
- "It is important to understand the balance between scope 2 and scope 3 emissions before deciding on how to approach reducing your emissions."
---

The first step in understanding how to reduce the emissions from your research or other work is quantifying the emissions from different sources so you can understand where action to reduce emissions can have the largest impact. Emissions from your use of HPC may or may not be a large component and there has been a lot of work recently from the HPC community to enable users to estimate their emissions.

So that we can compare emissions from HPC system use to other sources, we need to use a common, agreed framework to quantify emissions. The framework that is most widely used is the Greenhouse Gas (GHG) Protocol.

## Greenhouse Gas (GHG) Protocol

The [GHG Protocol](https://ghgprotocol.org/sites/default/files/standards/ghg-protocol-revised.pdf) is the most widely used and internationally recognised greenhouse gas accounting standard. Many organisations use the protocol and it provides the basis of emissions reporting for most countries (including the UK). Using the GHG protocol allows us to compare our emissions from use of HPC systems to other sources of emissions in a quantitative way.

The GHG protocol divides emissions into three scopes:

- Scope 1: Direct emissions from operations owned or controlled by the reporting organisation, such as on-site fuel combustion or fleet vehicles.
- Scope 2: Indirect emissions related to emission generation of purchased energy.
- Scope 3: Other indirect emissions from activities. Scope 3 emissions are typically split into two further categories: Upstream Emissions and Downstream Emissions:
  - Upstream Scope 3 Emissions: Includes all emissions from an organisation’s supply chain, e.g. emissions from manufacturing and shipping a product
  - Downstream Scope 3 Emissions: Emissions resulting from the use of a product, e.g. the electricity customers may consume when using your product.

Whether the emissions from electricity use on HPC systems are Downstream Scope 3 or Scope 2 really depends on who is computing the emissions and for what purpose. From the viewpoint of the hardware vendor who sells and manufactures the HPC system, the electricity use falls into Downstream Scope 3 emissions but for operators and users of the HPC system they would classified as Scope 2 emissions. As we are approaching this subject as a provider of HPC services we will always classify the emissions from our electricity use on HPC systems as Scope 2.

## Estimating emissions from an HPC system

We present the case study of ARCHER2 below but the mechanism for estimating emissions for any HPC system follows a similar process:

1. Estimate the total lifetime scope 3 (embodied emissions) of the HPC hardware by sourcing values from reports and vendor data sheets.
2. Divide the total scope 3 emissions by the total number of resource units available over the lifetime of the service (e.g. coreh, nodeh, GPUh) to obtain an emissions rate per resource unit (e.g. kgCO2e/coreh).
3. Audit the power draw by component across the whole HPC to understand which energy use can be measured on a per job basis and which energy needs to be added as overheads and what size those overheads may be.
4. Decide if you will use instantaneous carbon intensity of electricity generation to compute emissions from energy or if you will use an average value of some sort. The instantaneous values (e.g. from carbonintensity.org.uk) will allow for more accurate emissions estimates. Using an average value will make estimation easier and may be useful for getting a useful first pass to understand how your HPC emissions fit in the wider emissions generated from your work.

The HPC system you are using may already have values and tools available for estimating emissions. For example, on ARCHER2 you can estimate your emissions using tools installed on the system, see [ARCHER2 documentation](https://docs.archer2.ac.uk/user-guide/energy/#emissions).

### Scope 3 emissions

Scope 3 emissions from the ARCHER2 hardware have been estimated from a subset of the components that are expected to 
make up the majority of the emissions. Note that there is a large amount of uncertainty for scope 3 emissions due
to lack of high quality Scope 3 emissions data from vendors. In particular, the number used for the compute node
emissions is at the high end of estimated values and the actual value could be as much as 15% lower at around 
900 kgCO<sub>2</sub>e/node.

| Component | Count | Estimated kgCO<sub>2</sub>e per unit | Estimated kgCO<sub>2</sub>e | % Total Scope 3 | References |
|---|--:|--:|--:|--:|---|
| Compute nodes | 5,860 nodes | 1,100 | 6,400,000 | 84% | (1) |
| Interconnect switches | 768 switches | 280 | 150,000 | 2% | (2) |
| Lustre HDD | 19,759,200 GB | 0.02 | 400,000 | 6% | (3) |
| Lustre SSD | 1,900,800 GB | 0.16 | 300,000 | 4% | (3) |
| NFS HDD | 3,240,000 GB | 0.02 | 70,000 | 1% | (3) |
| Total | | | 7,320,000 | 100% | |

We then estimate the per-CU (nodeh) Scope 3 emissions by assuming a service lifetime of 6 years and
100% availability:

```
7,320,000 kgCO2e / (5,860 nodes * 6 years * 365 days * 24 hours) = 0.023 kgCO2e/CU
```
{: .output}

We use a value of **0.023 kgCO<sub>2</sub>e/CU** for ARCHER2.

> ## Extending the lifetime of the service improves the carbon efficiency
> 
> As one of the main parts of computing the scope 3 emissions rate is the HPC service lifetime
> one of the simplest ways that a HPC service operator can improve the scope 3 emissions
> efficiency is by extending the lifetime of the service.
{: .callout}

References:

1. [IRISCAST Final Report](https://doi.org/10.5281/zenodo.7692451)
2. Estimate taken from IBM z16™ multi frame 24-port Ethernet Switch Product Carbon Footprint
3. [Tannu and Nair, 2023](https://arxiv.org/abs/2207.10793)

### Scope 2 emissions

Scope 2 emissions from ARCHER2 are zero as the service is supplied by 100% certified renewable energy.
For information purposes we can calculate what the scope 2 emissions would have been if the energy
was not 100% renewable energy using the methodology described below.

We are aware that there is ongoing discussion in the sustainability community about the impact and
effectiveness of certified renewable energy contracts that are supplied through UK National Grid
connections. We are monitoring these discussions and taking advice from sustainability professionals
on how we report and estimate ARCHER2 emissions.

UK National Grid based scope 2 emissions are calculated using the compute node energy use for particular
jobs along with the carbon intensity of the South Scotland region of the UK National Grid at the start
time of the job. The carbon intensity is retrieved from the [carbonintensity.org.uk](carbonintensity.org.uk)
web API.

If the energy use of a job is not available (which happens occasionally due to, e.g. counter failures) then
the mean per node power draw from 1 Jan 2024 - 30 Jun 2024 on ARCHER2 is used to compute the energy
consumption. This corresponds to a value of 0.41 kW per node.

Estimates of power draw of individual components of ARCHER2 suggest that the compute node power draw makes up
around 85% of the system power draw so to estimate energy use by additional components we add
15% of the measured compute node energy.

| Component | Count | Loaded power draw per unit (kW)| Loaded power draw (kW) | % Total | Notes |
|---|--:|--:|--:|--:|---|
| Compute nodes | 5,860 nodes | 0.41 | 2,400 | 85% | Measured by on system counters |
| Interconnect switches | 768 switches | 0.24 | 240 | 9% | Measured by on system counters |
| Lustre storage | 5 file systems | 8 | 40 | 1% | Estimate from vendor |
| NFS storage | 4 file systems | 8 | 32 | 1% | Estimate from vendor |
| Coolant distribution units | 6 CDU | 16 | 96 | 3% | Estimate from vendor |
| Total | | | 2,808 | 99% | |

Current scope 2 grid based emission calculations estimates do not include overheads from the electrical
and cooling plant, these will vary with outside weather conditions at the data centre but are typically
less than 10%. As a conservative estimate, we add an additional 10% energy use to the total to 
account for plant overheads. 

The final energy calculation for a job is therefore:

1. Take measured compute node energy use from Slurm (or, if not available for that job use a per-node
   power draw of 0.41 kW to estimate energy use).
2. Add an additional 15% of this compute node energy use to estimate energy use by other components.
3. Add an additional 10% of the new total energy use to estimate energy use overheads from plant.

This energy consumption (in kWh) can then be used to compute the emissions from the job by multiplying 
the energy use by the carbon intensity (in kgCO2e/kWh) by the job energy use. In the tools used to 
estimate emissions on ARCHER2, we use the carbon intensity value for S. Scotland from the UK National Grid
at the start time of the job.

> ## How do HPC systems reduce HPC emissions?
> 
> As well as a producer of GHG emissions, HPC systems like ARCHER2 also contribute to reducing emissions. The main source of reduced emissions from services such as ARCHER2 is in the research that leads to new technology, policies and approaches to reducing emissions. Some examples include:
>
> - HPC services run the climate models that are used to provide evidence for setting emissions reductions policies and targets across the world.
> Research and modelling on HPC services leads to development of improved zero emission energy generation by, for example, modelling new wind turbine and wind farm designs.
> - Modelling to support the development of new energy storage technologies such as improved batteries. The emissions reductions from such activities are extremely difficult to quantify for a number of reasons so, at the moment, these are not factored in to the emissions estimates for ARCHER2.
> 
> As well as the research activities on the service leading to reductions in emissions, there are other activities that HPC services can potentially take. For example:
>
> - Using the waste heat generated by large scale HPC services as a heat source for homes, businesses or farming. For the ACF data centre where ARCHER2 is hosted we are looking for options on how to do this.
> - Incorporating environmental and biodiversity improvements into the service. For the ACF data centre (which is in a rural location) we have been working to improve the site biodiversity and improve habitats. Responsible carbon offset schemes could also potentially be used to reduce emissions if they were undertaken as part of the service.
{: .callout}

## Estimating emissions from your use of HPC systems

We will describe a simple scheme for getting a first, rough estimate for the emissions from your use of an HPC system. If this calculation shows that your HPC system use is likely to be a significant source of emissions within your wider activities then you can use the more detailed scheme described above that uses instantaneous carbon intensity values.

For the calculation you need:

- The amount of resource consumed 
- An estimate of the energy use per resource consumed (e.g. kWh/nodeh)
- An estimate of the average carbon intensity for the period of usage you are looking at
- An estimated value of scope 3 (embodied emissions) per resource consumed

The emissions for your use of the HPC system is then given by (assuming resources in nodeh)

- Scope 2 = (Resource consumed in nodeh) &times; (Energy use rate in kWh/nodeh) &times; (Carbon intensity in gCO2e/kWh)
- Scope 3 = (Resource consumed in nodeh) &times; (Scope 3 emissions rate in kgCO2e/nodeh)
- Total emissions = Scope 2 + Scope 3

## Reducing my emissions from use of HPC systems

Once you have estimated your emissions then how you start to reduce your emissions depends on whether scope 2 emissions dominate, scope 3 emissions dominate or they are roughly equal. 

All of the following discussion assumes you have a fixed amount of work you want to do. Obviously, a strategy that works in all cases is to reduce the amount of HPC resources you use. This may not be practical, but it does require all of us to remember that we have a responsibility consider carefully the carbon cost of any calculations we undertake and be confident that a calculation will do useful and meaningful work before we start it.

We outline a number of strategies for the different cases below. Which you undertake first will be driven by practical considerations such as scale of potential reduction and ease of implementation.

### Scope 2 emissions dominate

There are a number of different strategies to reduce your emissions in this case, these include:

- Improve the energy efficiency of your application
  - Could be by modifying the software to use more energy efficient algorithms
  - Could be by imposing a power cap (or CPU/GPU frequency cap) on the processors you are using
- Run your calculations only when the carbon intensity is lower - *tempoaral shifting*
  - You can obtain carbon intensity forecasts for the location of your HPC service from carbonintensity.org.uk
- Move your calculations to an HPC facility in a location with lower carbon intensity - *spatial shifting*

| UK Region | Mean 2024 CI (gCOe/kWh) | National DRI hosted in area |
|---|--:|---|
| NE England | 22 | DiRAC MI (COSMA) |
| S Scotland| 26 | ARCHER2, DiRAC ES (Tursa) |
| N Scotland | 30 | |
| NW England | 48 | |
| N Wales | 77 | |
| E England | 108 | DiRAC DI (DIaC/CSD3), AIRR (Dawn) |
| London | 125 | |
| W Midlands | 125 | |
| SE England | 135 | |
| Yorkshire | 135 | |
| S England | 186 | |
| E Midlands | 203 | DiRAC DI (DIaL) |
| SW England | 242 | AIRR (IsambardAI), Tier-2 HPC (Isambard3) |
| S Wales | 255 | |

### Scope 3 emissions dominate

Your aim is to increase the amount of output you get from each resource unit (e.g. nodeh) used irrespective of energy use.

- Improve the performance of your application
- Remove any power caps (or CPU/GPU frequency caps)
- Move your calculations to an HPC facility that has a lower emissions rate per amount of output for your use case - *spatial shifting*

### Scope 2 and scope 3 roughly equal

In this case you can use any and all of the strategies described above to reduce your emissions footprint.

## ARCHER2 emissions compared to other sources

Comparisons to other work activities: travel to conferences in USA and lab work.

1 year, ARCHER2 in S. Scotland:

|   | Emissions | Transatlantic flights | Person years of lab |
|---|---:|----:|---:|
| ARCHER2 total | 1,728,000 kgCO2e | 864 | 432 |
| Heaviest user | 53,600 kgCO2e | 27 | 13 |
| Average user | 1,800 kgCO2e | 0.9 | 0.5 |

1 year, ARCHER2 in S.W. England:

|   | Emissions | Transatlantic flights | Person years of lab |
|---|---:|----:|---:|
| ARCHER2 total | 6,274,000 kgCO2e | 3,140 | 1,570 |
| Heaviest user | 212,900 kgCO2e | 106 | 53 |
| Average user | 6,700 kgCO2e | 3.4 | 1.7 |

- Lab values include purchases, heating and electricity; 4,000 kgCO2e/person-year From: [https://pubs.rsc.org/en/content/articlehtml/2024/gc/d3gc03668e]
- Flights values 2,000 kgCO2e per return flight from London to New York. From: [https://www.clevel.co.uk/flight-carbon-calculator/] and [https://howbadarebananas.com/] 

### Other comparisons

Comparisons to other day-to-day activities.

1 year, ARCHER2 in S. Scotland:

|   | Emissions | Miles driven | 8 oz (225 g) steaks | 225 g portions of carrots |
|---|---:|----:|---:|---:|
| ARCHER2 total | 1,728,000 kgCO2e | 3,260,000 | 300,000 | 27,500,000 | 
| Heaviest user | 53,600 kgCO2e | 101,000 | 9,200 | 851,000 |
| Average user | 1,800 kgCO2e | 3,400 | 310 | 29,000 |

- 1 mile of average UK car = 0.530 kgCO2e.  From: [https://howbadarebananas.com/] 
- 8 oz (225 g) raw steak from UK beef herd = 5.8 kgCO2e. From: [https://howbadarebananas.com/] 
- 225 g portion of UK carrots = 0.063 kgCO2e. From: [https://howbadarebananas.com/] 
