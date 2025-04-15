# Strasbourg

Most of the documentation below is adapted from the one from Nantes.

The pipeline makes it easy to create synthetic populations and simulations
for other regions than Île-de-France. In any case, we recommend to first
follow instructions to set up a [synthetic population for Île-de-France](../population.md)
and (if desired) the [respective simulation](../simulation.md). The following
describes the steps and additional data sets necessary to create a population and
simulation for **Strasbourg** and its surrounding department Bas-Rhin.

## Additional data

### A) Buildings database (BD TOPO)

You need to download the region-specific buildings database.

- [Buildings database](https://geoservices.ign.fr/bdtopo)
- In the sidebar on the right, under *Téléchargement anciennes éditions*, click on *BD TOPO® 2022 GeoPackage Départements* to go to the saved data publications from 2022.
- The data is split by department and they are identified with a number. For the Bas-Rhin (Unterelsàss) department around Strasbourg, download:
  - Bas-Rhin (67)
- Copy the *7z* file into `data/bdtopo_strasbourg`.

### B) OpenStreetMap data

Only if you plan to run a simulation (and not just generate a synthetic population),
you need to obtain additional data from OpenStreetMap.
Geofabrik provides a cut-out for [Alsace](https://download.geofabrik.de/europe/france/alsace.html): [alsace-220101.osm.pbf](https://download.geofabrik.de/europe/france/alsace-220101.osm.pbf).
Download the file in *.osm.pbf* format and put the file into the
folder `data/osm_strasbourg`.

### C) GTFS data

Again, only if you want to run simulations, the digital transit schedule is required.
Unfortunately, there is no consolidated GTFS schedule avaiable for the region of interest. Hence,
it is necessary to collect all relevant GTFS schedules one by one.
Here, we provide a selection of links, which is not necessarily exhaustive:

- [CTS Strasbourg](https://transport.data.gouv.fr/datasets/donnees-theoriques-gtfs-et-temps-reel-siri-lite-du-reseau-cts)
- [Réseau Interurbain Fluo 67](https://transport.data.gouv.fr/datasets/fr-200052264-t0049-0000-1)
- [Ritmo Haguenau](https://transport.data.gouv.fr/datasets/fr-200052264-t0008-0000-1)
- [ELSA Sélestat Alsace Centrale](https://transport.data.gouv.fr/datasets/fr-200052264-t0008-0000-1)
- [SNCF TER](https://ressources.data.sncf.com/explore/dataset/sncf-ter-gtfs/information/)
- [SNCF TGV](https://ressources.data.sncf.com/explore/dataset/horaires-des-train-voyages-tgvinouiouigo/information/)

SNCF has announced they will merge their GTFS feeds in the next months, the links will change.

Download all the *zip*'d GTFS schedules and put them into the folder `data/gtfs_strasbourg`.

### D) Adresses database (BAN)

You need to download the region-specific adresses database :

- [Adresses database](https://adresse.data.gouv.fr/data/ban/adresses/latest/csv/)
- Click on the link *adresses-67.csv.gz*
- Copy the *gz* file into `data/ban_strasbourg`.

### Overview

Afterwards, you should have the following additional files in your directory structure:

- `data/bdtopo_strasbourg/BDTOPO_3-0_TOUSTHEMES_GPKG_LAMB93_D067_2022-03-15.7z`
- `data/ban_strasbourg/adresses-67.csv.gz`

*Only for simulation:*

- `data/osm_strasbourg/alsace-latest.osm.pbf`
- `data/gtfs_strasbourg/google-transit.zip` (GTFS from CTS Strasbourg)
- `data/gtfs_strasbourg/fluo-grand-est-ccselestat-gtfs.zip`
- `data/gtfs_strasbourg/fluo-grand-est-haguenau-gtfs.zip`
- `data/gtfs_strasbourg/export-ter-gtfs-last.zip`
- `data/gtfs_strasbourg/export_gtfs_voyages.zip`

Note that the file names may change slightly over time as GTFS schedule are
updated continuously.

## Generating the population

An adapted version of the `config.yml` for Strasbourg, is available as `config_strasbourg.yml`.

To generate the synthetic population, the `config.yml` needs to be updated.
By default the pipeline will filter all other data sets for the
Île-de-France region. To make it use the selected region, adjust the
configuration as follows:

```yaml
config:
  # ...
  regions: []
  departments: [67]
  # ...
```

This will make the pipeline filter all data sets for the department Bas-Rhin (67).

Finally, to not confuse output names, we can define a new prefix for the output files:

```yaml
config:
  # ...
  output_prefix: strasbourg_
  # ...
```

You can now enter your Anaconda environment and call the pipeline with the
`synthesis.output` stage activated. This will generate a synthetic population
for Strasbourg and surroundings.

## Running the simulation

To prepare the pipeline for a simulation of Strasbourg, the paths to the OSM data sets and to the GTFS schedule must be adjusted explicitly:

```yaml
config:
  # ...
  gtfs_path: gtfs_strasbourg
  osm_path: osm_strasbourg
  ban_path: ban_strasbourg
  bdtopo_path: bdtopo_strasbourg
  # ...
```

Note that the pipeline will automatically cut GTFS and OpenStreetMap data
to the relevant area (defined by the filter above) if you run the simulation.

To test the simulation and generate the relevant MATSim files, run the pipeline
with the `matsim.output` stage enabled.
