# IMProToo - Improved Mrr Processing Tool



IMProToo is an improved processing method for Micro Rain radar. It is especially suited for snow observations and provides besides other things effective reflectivity, Doppler velocity and spectral width. The method features a noise removal based on recognition of the most significant peak and a dynamic dealiasing routine which allows observations even if the Nyquist velocity range is exceeded. The software requires MRR "raw data", it does not work with Metek's standard products MRR "Averaged Data" or "Processed Data".

Please note that this software was developed for observations at low SNR ratios such as snow, drizzle or light rain. Heavy rain, especially in combination with strong turbulence, might give wrong results.

The software can be used under the GPL license

## How does it work 

The routine is described in 
Maahn, M. and Kollias, P.: Improved Micro Rain Radar snow measurements using Doppler spectra post-processing, Atmos. Meas. Tech. Discuss., 5, 4771-4808, doi:10.5194/amtd-5-4771-2012, 2012. http://www.atmos-meas-tech-discuss.net/5/4771/2012/amtd-5-4771-2012.html

Please quote the article if you use the routine for your publication.

## How to install

The software is developed for python 2.7 and should run on any recent Linux system (and most likely also Mac OS X). Windows is currently not supported, but probably only minor changes are necessary.

The following python packages are required:
  * numpy
  * matplotlib (for plotting only)
  * http://code.google.com/p/netcdf4-python/|netcdf4-python OR python-netcdf (for saving the results only)


An installation routine is not provided, Instead, save the included files in your working directory and import the package using "import IMProToo". If you want to save the files in another directory, you have to add this directory to your python path (http://stackoverflow.com/questions/3108285/in-python-script-how-do-i-set-pythonpath). If you have root permissions, you can also put the files into your python library directory (usually /usr/lib/python).

## How to use

To use the toolkit, start python and import it:
```
  import IMProToo
```

read the raw data file (can be gzip-compressed)
```
  rawData = IMProToo.mrrRawData("mrrRawFile.mrr.gz")
```

create the IMProToo object and load rawData
```
  processedSpec = IMProToo.MrrZe(rawData)
```

if needed, average rawData to 60s
```
  processedSpec.averageSpectra(60)
```

all settings (e.g. creator attribute of netCDF file, dealiasing) are available in the 'processedSpec.co' dictionary and must be set before calculating Ze etc. See the source code for a description of the settings.
```
  processedSpec.co["ncCreator"] = "M.Maahn, IGM University of Cologne"
  processedSpec.co["ncDescription"] = "MRR data from Cologne"
  processedSpec.co["dealiaseSpectrum"] = True    
```
  
calculate Ze and other moments
  processedSpec.rawToSnow()

write all variables to a netCDF file.
```
  processedSpec.writeNetCDF("IMProToo_netCDF_file.nc",ncForm="NETCDF3_CLASSIC")
```


## Questions
In case of any questions, please don't hesitate to contact Maximilian Maahn: mmaahn_(AT)_meteo_DOT_uni-koeln_DOT_de
