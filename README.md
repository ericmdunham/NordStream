# Nord Stream Model

This repository contains code to model the 2022 Nord Stream pipeline events and underwater explosions. Underwater explosions are comprised of an initial blast, created as the solid reactant is converted to reaction product gases, followed by oscillations of the gas bubble. For Nord Stream events, this explosion model is combined with a model for gas discharge from the ruptured pipeline, which adds mass and energy to the bubble. Changes in the volume of the gas bubble are converted to a seismic moment tensor. The moment rate is convolved with seismic wave Green’s functions to produce regional seismograms.

## Dependencies

**Seismic data**: Seismic data are available [here](https://doi.org/10.18159/NordStreamV1).

**Green’s functions**: Green's functions, computed using [pyrocko](https://pyrocko.org/), are available [here](https://doi.org/10.25740/jk284nq7928). The steps used to generate these Green's functions are provided in `GreensFunctions/GreensFunctionInstructions.txt`.

**miniseed file reader**: The seismic data and Green's functions are in the miniseed file format. These files are read using `rdmseed.m` from this [repository](https://github.com/IPGP/mseed-lib) or from [here](https://www.mathworks.com/matlabcentral/fileexchange/28803-rdmseed-and-mkmseed-read-and-write-miniseed-files).

## Installation

1. Clone this repository.
2. Download the seismic data. Copy (or link) the files to `seismicData`.
3. Copy miniseed file reader `rdmseed.m` in the main project folder (or add the folder containing the file to your MATLAB path).
4. Download the Green's functions. Copy (or link) the files to `GreensFunctions`.

## Modeling

### Validation on underwater explosions

1. Run `runValidation.m` to run the model and store results in `.mat` files in `validation`.
2. Run `plotValidation.m` to load results from `.mat` files in `validation` and make plots, which are stored in `validationPlots`. Disable saving of plots (but they will still appear in figure windows) by setting `savePlot = false` at the top of the script.

### Source depth affects seismic amplitude – yield relationships

1. Run `runVaryDepthExplosion.m` to run the model for an explosion at different depths and make plots of the results.

### Validation on Balticconnector pipeline rupture event

1. Run `runBC.m` to run the model and store results in `.mat` files in `validation`.
2. Run `plotBC.m` to load results from `.mat` files in `validation` and make plots, which are stored in `validationFigures`.

### Nord Stream events

1. Run `runCompareMassAll.m` to run the model and make plots for the SE event and NE events at three different leak locations. It first calls `runCompareMass.m` to run the model and store results in `.mat` files in `compareMass`. Then it calls `plotCompareMass.m` to load results from `.mat` files and make plots, which are stored in `compareMassFigures`. The `plotCompareMass.m` script calls `plotCompareMassSolution.m` to plot the bubble solution and `plotCompareMassSeismograms.m` to plot seismograms and spectra. Disable saving of plots (but they will still appear in figure windows) by setting `savePlot = false` at the top of the `runCompareMassAll.m` script.
2. Run `plotCompareMassWithWithoutGasDischarge.m` to load results from `.mat` files in `compareMass` and make plots comparing seismograms and spectra from models with and without gas discharge. These are stored in `compareMassFigures`.
3. Step 1, which calls `plotCompareMass.m`, only produces seismograms for a single event. This is sufficient for Nord Stream SE, but Nord Stream NE was comprised of at least three events at different locations. Run `plotNorthSeismograms.m` to load results from `.mat` files in `compareMass`, use superposition to create seismograms from multiple events, and make plots, which are stored in `compareMassFigures`.

### Additional scripts

* `methane.m` plots methane properties as a function of pressure.
* `plotDischargeDependence.m` plots mass and energy discharge rates as a function of pressure for different pipeline inner diameters.
* `plotSelfSimilarRarefaction.m` plots the self-similar rarefaction solution used to calculate mass and energy discharge rates.

### More information on the code

* `NordStream.m` is the class containing the modeling parameters and functions.
* `runModel.m` is the function that initializes the class, runs the model (for explosions only, explosions with gas discharge, and gas discharge only), and convolves the moment rate with the Green's functions to generate synthetic seismograms. This is done for one set of parameters. This function is called by `runCompareMass.m`, `runValidation.m`, and `runBC.m`, for example.
* `loadGF.m` and `loadGF_pyrocko.m' are used to load the Green's functions.
* `loadSeismicData.m` loads the seismic data. `loadStations.m` loads station information.
* There are a few other helper functions for things like windowing, Fourier transforms of real-valued time series, and computing the envelope of a seismogram.

***

## Author

Eric M. Dunham (edunham@stanford.edu), Department of Geophysics, Stanford University

## Publication

This code was used in the following publication:

Lund, B., Dunham, E. M., Steinberg, A., Köhler, A., Alvizuri, C., Hupe, P., Dahl-Jensen, T., Heimann, S., Eggertsson, G., Roth, M., Schmidt, P., Shomali, H., Larsen, T., Rinds, N., Voss, P., Dando, B., Goertz-Allmann, B., Schweitzer, J., Ceranna, L., Gestermann, N., Plenefisch, T., Weidle, C., Seismic investigation of the 2022 Nord Stream pipeline sabotage, submitted.
