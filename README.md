# CLASP_LES_heterogeneity
Code for GRL paper "Scales of Surface Heterogeneity Affecting the Daytime Convective Atmosphere over Land"

Five Jupyter notebooks are included: 3 that perform data processing on the LES model 
output available from: [https://doi.org/10.5281/zenodo.8241941](https://doi.org/10.5281/zenodo.8241941), 
and two that produce the figures in the peer-reviewed journal paper:

Dirmeyer, P. A., F. M. Hay-Chapman, J. S. Simon, and N. W. Chaney, 2024: 
Scales of surface heterogeneity affecting the daytime convective atmosphere over land. _Geophys. Res. Lett._

__1.__ `ancillary_LES_data.ipynb` calculates and writes out additional 2D ancillary fields for each hour of each LES simulation:
* stability regime
* cloud base (pressure and height AGL)
* PBL top (pressure and height AGL) calculated using theta and theta_v
* LCL pressure, height, temperature
* LFC pressure, height, temperature
* EL pressure, height, temperature
* CIN
* CAPE

The expanded variable list is written to pbl2c_YYYYMMDD_SS.nc4
* YYYY = 4-digit year
* MM   = 2-digit month
* DD   = 2-digit day
* SS   = set of lower boundary conditions
    * 00   = heterogeneous BCs from HydroBlocks (HET)
    * 01   = homogeneous (HOM; domain mean applied at all grid cells) 

__2.__ `spatial_PSD_data.ipynb` reads the hourly LES output files and ancillary data files:
* Calculates the power spectral density (PSD) as a function of wavelength (wavenumber), hour, and for the 3-D fields, model level.
* For each variable, the output has dimensions of time (hour), bottom_top (i.e., model level, if 3D) and wavelength.
* Writes out NetCDF files `fr2_PSD_##.nc4` where `##` is the lower boundary condition code `00` or `01`
    * 00   = heterogeneous BCs from HydroBlocks (HET)
    * 01   = homogeneous (HOM; domain mean applied at all grid cells)

__3.__ `statistics_PSD_data.ipynb` reads the hourly PSD data from `fr2_PSD_##.nc4` (both `00` and `01`)
* Calculates a number of statistics as a function of hour and wavelength:
    * The exp(mean(log)) across cases of PSD for variables in each HET and HOM sets
    * exp(mean(log(HET/HOM))) across cases of PSD for variables
    * Log variance across cases of PSD for variables in each HET and HOM sets
    * Correlation (HOM vs HET) of log(PSD) for specific variables across cases
    * The relative entropy (HOM vs HET) across cases of PSD for variables
* Writes out `fr2_stats2_PSD.nc4`

__4.__ `heterogeneity_correlation_plot.ipynb` generates Figure 1.

__5.__ `statistics_PSD_plots.ipynb` generates all other figures.
