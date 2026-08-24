SIH26143 — "Maritime Oil Spill Intelligence"

--> What we are trying to solve

An oil spill at sea is not only a detection problem. Once a possible spill is observed, one of the important questions is where it came from and which vessels were operating around the affected area at the relevant time.

Our idea is to connect these two pieces of information instead of treating them separately.

We are building a prototype that uses satellite imagery to identify probable oil-spill regions and AIS vessel movement data to analyse vessels that may be associated with the event.

The final system is intended to help an investigator move from:

"There may be an oil spill here."

to:

"These are the vessels that were operating in the relevant area and time, and this is the evidence used to rank them."

-----------------------------------------------------------------------------------------------------------------------------
-----------------------------------------------------------------------------------------------------------------------------
-=> How the system will work

At a high level, the workflow is:

Satellite imagery  
→ image preprocessing  
→ probable oil-spill detection  
→ spill location and extent  
→ AIS vessel filtering  
→ spatial + temporal analysis  
→ candidate ranking  
→ map-based investigation view

The attribution stage is deliberately designed as a ranking mechanism. A high score means that the available evidence is more consistent with a vessel being associated with the event; it is not treated as proof of responsibility.

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------
--> Main components
 1. Satellite analysis

We will begin with Sentinel-1 SAR imagery because the project needs a source that can support maritime observation under varying lighting and weather conditions.

The first milestone is simple:

> Given a suitable satellite scene, identify the region that is likely to contain an oil spill.

The preprocessing and model approach will be finalized after evaluating the available imagery and labelled data rather than assuming a particular model in advance.

### 2. Oil-spill detection

The detection module will produce a probable spill region rather than only a yes/no classification.

Where the data allows it, we will retain:

- spill mask or region
- geographic coordinates
- estimated extent
- model confidence
- image acquisition time

These outputs will become inputs to the correlation stage.

3. AIS vessel analysis

AIS data provides the movement history of vessels.

For the prototype, the relevant information will include vessel identity, timestamp, position and, where available, movement characteristics such as speed and heading.

Instead of considering every vessel equally, we will first restrict the search to vessels that are relevant to the spill's location and time window.

 4. Vessel correlation

This is the main analytical layer of the project.

For every relevant vessel, we will examine factors such as:

- distance from the detected spill
- compatibility with the estimated time window
- trajectory relative to the spill
- heading/course
- movement behaviour around the event

The result will be a ranked list of candidate vessels together with the factors contributing to their score.

5. Investigation dashboard

The final interface will bring the results together on an interactive map.

The user should be able to see:

- detected spill region
- satellite context
- vessel trajectories
- candidate vessels
- attribution scores
- supporting evidence

The aim is to make the result understandable without requiring the user to inspect raw datasets manually.


 --> Initial technology direction

We are currently considering:

Data / geospatial
- Python
- GeoPandas
- Rasterio
- Shapely
- NumPy / Pandas

Machine learning
- PyTorch
- OpenCV
- scikit-learn where appropriate

Backend
- FastAPI

Frontend
- React
- Leaflet or MapLibre

Storage
- PostgreSQL/PostGIS if the prototype requires persistent geospatial querying

These choices are not being treated as fixed requirements. We will keep the stack as small as possible and only introduce a technology when it solves an actual project need.


 --> What we are validating first

Before building the complete system, we are checking three things:

1. Whether suitable satellite scenes and oil-spill labels are available for model development.
2. Whether usable historical AIS data can be obtained for the prototype.
3. Whether the two data sources can be brought into a common geographic and temporal reference.

These checks will determine the final implementation strategy.

--> Current status

Stage: Initial setup and data validation

Completed:
- Project repository created
- Team workspace created
- Initial development tasks divided among team members

Next milestones:
- Validate satellite data
- Validate oil-spill training/evaluation data
- Validate AIS data
- Build independent proof-of-concept pipelines
- Connect the pipelines
- Develop vessel ranking logic
- Build the investigation dashboard

--> Important design principle

The system should provide evidence and ranked candidates, not make unsupported claims.

Where uncertainty exists, it should be visible to the user.
