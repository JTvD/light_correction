# PAR Sensors, Theory and Calibration
A write up about PAR sensors and how they compared to values from other PAR sensors.
It is not meant to check the quality of a PAR sensor or calibarate, validate them. 
This requires an advanced lab setup, with the zenith angle and spectrometer or even better monochromator and special light sources. The amounth of time that goes into this was not deemed worth it, yet they are mentioned in the possible follow up steps. 

## What is a PAR Sensor?
Photosynthetically active radiation (PAR) Sensors are photodiode sensors that attempt to measure the of region of light from 400-700 nm. In the solar spectrum this is the region of light that drives photosynthesis, below 400 nm not much of light gets through the atmosphere, while over 700 nm the light does not have enough energy to drive Photosynthesis.
To do this, most PAR sensors are composed of three elements. A filter that blocks all light expect the PAR region, a photodiode to collect light, and an electronic board that converts the electric signal into a PAR value. The PAR value can be expressed in two ways photosynthetic photon flux (PPF) and yield photon flux (YPF). PPF values all photons from 400 to 700 nm equally and so corresponds to just the intensity of light on the diode, while YPF weights photons in the range from 360 to 760 nm based on a plant's photosynthetic response. To use YPF the spectrum of light incident on the sensor must be known.

We need to calibrate sensors due to differences in all three elements of the PAR sensor. Filters made by different companies will allow in different amounts of the spectrum, the photodiodes will convert photons to electronic signal differently, and the calculation to turn the signal into PPF is proprietary to each company. To do this we need a reference to calibrate all of these sensors to.

## Why is the Quantum Sensor Special?
Quantum Sensors (QS) as used by companies like Apogee and Licor are a special type of PAR sensor. These are sensors directly measure the YPF instead of the PPF and also have extremely precise filters measuring the region from 400-700 nm with little error and extremely low noise. 
Because of that we can use these sensors to calibrate other sensors to a high degree of accuracy for specific light source. 

## How Do I Calibrate sensor?	
First, Begin by selecting the light source you want to calibrate the sensors under. The procedure is slightly different if you are calibrating for an electric light vs the sun. If you want to operate the par in both settings you need to perform both calibrations.

### Solar Calibration
Make sure you have an area of light that is uniform and that no external light is incident on the sensors. Before you begin you must wait for a clear day where you have at least 6 hours of cloudless skies.
1.	Place the sensors to calibrate outdoors on a level surface pointed straight into the sky. 
2.	Place the reference sensor as close to the other sensor as possible, at the same altitude. Do not allow Shadows on either of the sensors.
3.	Set up all sensors to record data at the same interval, we chose to average over 5 minute periods.
4.	Let the sensors record from minimum to maximum Solar irradiance , (sunup-noon, noon-sunset) At least 3 of these cycles must be measured.
5.	Export this data into an CSV

### Electronic Light Calibration 
Make sure you have an area of light that is uniform and that no external light is incident on the sensors. This is easiest to do at night time or in a darkened room. 
1.	Place the sensors to calibrate on a level surface pointed straight into the light, if possible, set them in the centre of the light beam. 
2.	Place the reference sensor as close to the other sensor as possible, at the same altitude. Do not allow Shadows on either of the sensors.
3.	Set up all sensors to record data at the same interval, we chose to average over 5 minute periods.
4.	Let the sensors record for three periods of on/off. If the light intensity changes in a cycle.  At least 3 of these cycles must be measured.
5.	Export this data into an CSV

### Create the calibration?
An easy way to align two sensors is to use a function to translate values from one to another, in a for of y = ax + b, or higher order function.
There are two examples given in `fit_sensors.ipynb`, rediduals and the RMSE can be used to check the fit.

## How Do I Apply this Calibration?
When exporting data from the sensors this data is raw, and must be transformed to be compared to other sensors. Transform each data to calibrated by multiplying the value from the sensor by a and adding b. The data can now be compared to each other.
See `apply_correction.ipynb` for an example.

## Can we improve this?
Of course we can. 
The Easiest way to improve the PAR data and ensure reading are comparable is to buy only high end sensors and have them recalibrated regularly. These sensors once Calibrated to each other can be used in any lighting condition.

Second, the current limitations of the calibration described in this document is that it requires every calibration with every light source and does not work well with mixing light sources such as sun+LEDS. 
To create a more flexible calibration we need to be able to characterize the filter, and Photodiode response for each sensor. To do this we would take each sensor and place it in front of a monochromator and then record the response of the device at every wavelength from 300-900 nm. This would produce a Quantum Response Curve for each sensor that can be compared to the QS. If the spectra of incident light is known then the by taking the area of light in PAR and that outside would give us a more robust Correction for any possible light source.  
This will take far more time and effort, but will allow us to use even low cost sensors like a QS.

