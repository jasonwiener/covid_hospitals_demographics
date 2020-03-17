## Hospital, population, nursing center data

This repository is a project to present and join datasets pertinent to the COVID-19 pandemic at the county level: hospital location and capacity, nursing home location and capacity, and county-level population estimates by age.

A simplified state-level view of this with only population breakouts for 65+ is available [here](https://docs.google.com/spreadsheets/d/1XC0SfpPgYkhLPe4CeXDKs3sgVw8Cbcqa3MpEt8tUQcY/edit#gid=0). The source data for statewide hospital beds is [here](https://www.ahd.com/state_statistics.html).

In general this repo is trying to follow the datakit repo convention, it isn't *actually* a datakit repo but may become one at some point. 


## Data

Each of the datasets is documented by the readme file in it's respective folder. In general the output files are in /data/processed/. 

These are the main output files

### Hospital-level bed data

CSV: [hospital_data.csv](https://github.com/jsfenfen/covid_hospitals_demographics/blob/master/data/processed/hospital_data.csv) ;  Shapefile [hosp_geo](https://github.com/jsfenfen/covid_hospitals_demographics/tree/master/data/processed/hosp_geo) 

(The shapefile leaves out [one hospital](https://data.medicare.gov/resource/xubh-q36u/row-hgvv.mh7i-bzfv) in Puerto Rico.)

This has basic hospital information, as well as bed counts from the 2017 hospital cost reports. The most recent filing was used (and the report number is available in the file). These come from page 9 column 2 of this [original form](https://www.cms.gov/Regulations-and-Guidance/Guidance/Manuals/Paper-Based-Manuals-Items/CMS021935) from 2017.  

There's an awesome [python notebook](https://github.com/jsfenfen/covid_hospitals_demographics/blob/master/data/analysis/HospitalICUBeds_2017.ipynb) written by [Erin Petenko](https://github.com/epetenko/) that makes a little clearer how to navigate this data.

The information in the downloadable file comes from the following lines. The documentation is a little hard to follow, see the instructions for completing this form on [p. 62 here](https://github.com/jsfenfen/covid_hospitals_demographics/blob/master/data/source/cost_reports/HOSPITAL2010-DOCUMENTATION/R15P240.pdf). 

Here are the bed numbers used, the variable names in **bold**

- **acute_beds** Adult/Pediatric Acute Care Beds 00700
- **icu_beds** Intensive Care Beds 00800
- **coronary_beds** Coronary Care Beds 00900
- **burn_beds Burn** Intensive Care Units 01000
- **surg\_icu_beds** Surgical ICU Beds 01100
- **oth\_spec\_beds** Other Specialty Beds 01200

The total of all of the above bed types is given, *roughly*, by:

- **total\_med\_beds** Total Beds 01400

Additional bed types

- **subprovider\_ipf\_beds**  01600
- **subprovider\_irf\_beds**  01700
- **subprovider\_oth\_beds**  01800
- **skilled\_nursing\_beds**  01900
- **nursing\_fac\_beds**  02000
- **oth\_longterm\_beds** 02100
- **hospice\_beds**  02400

The sum of total\_med\_beds and all additional bed types is given by

- **all\_beds** All Beds 02700



Military hospitals with an id ending in F are missing data. Psychiatric hospitals are not included.

The hospital's provider number should correspond to the provider number in the next file.

### Geocoded hospital and nursing home locations 

[hospital\_gen\_info\_geocoded\_final.csv](https://github.com/jsfenfen/covid_hospitals_demographics/blob/master/data/processed/hospital_gen_info_geocoded_final.csv)

This data comes from CMS hospital compare and nursing home compare and have on institution per line.

Suggested reading: ["COVID-19 story recipe: Analyzing nursing home data for infection-control problems"](https://source.opennews.org/articles/covid-19-story-recipe-analyzing-nursing-home-data/), Source, Mike Stucka, 3/16/20

CMS provides spatial data for most of these, but I've  added it to rows that were missing it (with google's geocoder). In these rows "geocode_flag" = 1 and the accuracy is as given by google. "ROOFTOP" is best, "RANGE INTERPOLATED" is next, for more info see google's [writeup](https://developers.google.com/maps/documentation/geocoding/intro)./

Todo: add the county fips codes, the names as text or they could be spatially joined. 

[nh\_gen\_info\_geocoded\_final.csv](https://github.com/jsfenfen/covid_hospitals_demographics/blob/master/data/processed/nh_gen_info_geocoded_final.csv) Is a file of CMS nursing home compare. Lat and lngs were added where they were missing; where this occurred geocode_flag = 1.

Todo: add the county fips codes. 


## Census Data


County-level population age data comes from the Annual Estimates of the Resident Population for Selected Age Groups by Sex for the United States, States: April 1, 2010 to July 1, 2018 from the [2018 Population Estimates](https://factfinder.census.gov/faces/tableservices/jsf/pages/productview.xhtml?src=bkmk).

The full download is rather extensive, the file [age_breakout.csv](https://github.com/jsfenfen/covid_hospitals_demographics/blob/master/data/processed/age_breakout.csv) contains just the geographic info and a selection of ages:

- GEO_id [ full FIPs code ]
- GEO_id2 [ county FIPs code ]
- GEO.display-label. [ County name ]
-est72018sex0_age999 [ County total, both sexes ]
- est72018sex0_age50to54 [ 2018 est, age 50-54 ]
- est72018sex0_age55to59 [ etc. ]
- est72018sex0_age60to64
- est72018sex0_age65to69
- est72018sex0_age70to74
- est72018sex0_age75to79
- est72018sex0_age80to84
- est72018sex0_age85plus [Age 85 and older ]




There's also a django app for doing more in-depth geographic work, although it's incomplete. 


## Stories

If you're able to use this in your work, or have relevant data to add, please let us know. 

Portland Tribune, 3/14 ["As number of virus cases grows, Oregon has lowest hospital bed rate in U.S."](https://pamplinmedia.com/pt/9-news/456432-372245-as-deluge-approaches-oregon-has-lowest-hospital-bed-rate-in-us).

## Contributors

Jacob Fenton, [PublicAccountability.org](https://publicaccountability.org); Erin Petenko  [VTDigger](https://vtdigger.org/)







