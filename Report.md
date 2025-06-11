## 1. Introduction
- We perform an NDVI analysis of Northern Africa (mostly Morocco) and Southern Europe (parts of Portugal, Spain, Andorra and France) using radiance data obtained from Sentinel3, and visualize it, performing a preliminary analysis of the data.
- Dataset used: Level-1B Sentinel3 SLSTR, bands S2 and S3, containing radiance in the red and infrared channels respectively.
- Purpose of NDVI analysis: To understand vegetation health across a geographical region, with high values of NDVI (>0.5) reflecting dense forests/healthy vegetation and low values (~0.2-0.5) reflecting vegetation stress, and values lower than that indicative of barren land/other non-vegetated surfaces.

## 2. Methodology
- Data preprocessing steps: Converted radiance to reflectance using the formula -
$$
R_{\mathrm{TOA}}(\lambda) = \frac{\pi \times L_{\mathrm{TOA}(\lambda)}}{E_0(\lambda)cos(\theta)}
$$
where $R_\mathrm{TOA}(\lambda)$ is the reflectance at wavelength $\lambda$, $L_{\mathrm{TOA}}$ is the top of the atmosphere radiance. $E_0$ is the solar irradiance and $\theta$ is the solar zenith angle. 
- Then used the cloud flag in order to mask all cloud pixels, and cleaned the solar zenith angle data to get rid of the nan values.
- Dask parallelization approach: After reading the data with xarray, we chunk it to sizes appropriate for the total dataset and machine specs (after monitoring it using Dask's dashboard interface). We then perform the conversion to reflectance and NDVI computation in a Dask-compatible way, ensuring scalability to larger datasets. We also observe the memory and CPU usage with the dashboard, to support troubleshooting.

## 3. Results
![](Results/ndvitiles.png)

#### Interpretation: 
- The NDVI plot reflects that the Northern part of Africa contains significant areas of barren land (start of the Sahara desert), with small traces of vegetation towards Morocco and along the coast of the Mediterranean. Small Regions of near yero values of NDVI are seen near the coast of Spain and Morocco (over the Mediterranean Sea), this could be the result of cloud contaminated/influence water.
- The vegetation cover gradually increases into mainland Europe, extending to parts of France. Some of the data in Andorra and middle/south of France is heavily affected by cloud, making it difficult to obtain much insights from the given data.
- The overall low values of NDVI for Spain and Portugal could be explained by the fact that the data is from June 2019, which were summer months when Europe was experiencing a heat wave.


## 4. Challenges & Lessons Learned
- After the initial computation (with simply scaling the SZA dataset), some of the data in the reflectance image was lost, because of ignoring nan values. After looking at the SZA dataset, I observed that almost all of the values in the last few columns were nans, which were causing this data loss. I then clipped the SZA array as a first order approximation, before interpolating it to get a complete map. 
- I troubleshooted with Dask to understand how to make the code scalable for bigger data, by first computing NDVI values for a smaller dataset without Dask, and then computing it for the whole dataset with parallel computing and comparing it to the initially computed values.

## 5. Conclusion
- Regarding the NDVI, we do find spatial patterns associated with known vegetation trends in regions of Northern Africa and Southern Europe. Some of the data is obscured by cloud, restricting interpretation.
- Using DASK, I was able to significantly reduce the processor and memory load, and runtime of my code. This makes the code efficiently scalable for larger datasets.
- By using good documentation practices, the produced code is a reproducible source for Sentinel3 SLSTR data processing and provides a basis for other indices to be calculated.
