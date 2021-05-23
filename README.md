# GEE_data_extraction

This repository provides the tools and codes to extract data from Google Earth Engine (GEE) datasets using Python. I provide a sample code for extracting data from monthly, daily, and hourly datasets which can be replicated for other datasets in GEE.

Google provides useful resources on GEE in python on the developments [webpage](https://developers.google.com/earth-engine/guides/python_install_) and the Earth Engine API github [repository](https://github.com/google/earthengine-api/tree/master/python).

# Steps
1. sign up for Google Earth Engine: https://signup.earthengine.google.com/#!/
    * It only takes a moment to sign up, but it can take over a day or two for GEE to approve
2. Determine which GEE dataset you are interested in using, explore GEE data catalog here: https://developers.google.com/earth-engine/datasets/catalog
3. import the Earth Engine API and initialize it
 ```
      import ee
      ee.Authenticate()
      ee.Initialize() 
 ```
4. Pull the coordinates for the sites you are interested in
   * method 1: manual longitude and latitude pulling
     ```
     #listing sites between Guilin and Dongfang in China and creating a list of longitute (lons), latitutes (lats), and names of sites (ids)
      
     lons = np.array([110.533096 , 109.786026 , 109.192765 , 111.324112 , 109.829972 , 109.082901])

     lats = np.array([ 25.842641 ,  23.708141 ,  22.231373  ,  21.783198 , 20.779974 , 18.857666])

     ids = np.array([ 'Guilin','Laibin' ,'Qinzhou','Maoming','Leizhou','Dongfang'])
     ```

   * method 2: creating a transect that interpolates between two points
     ```
     #input a starting point - Guilin
      lat1 = 25.842641
      lon1 = 110.533096

      #input an ending point - Dongfang
      lat2 = 18.857666
      lon2 = 109.082901

      #specify how many sites you want along your transect- this number should be small (<10 probably), because GEE will cut you off
      numsites = 10

      lats = np.linspace(lat1, lat2, num=numsites, endpoint=True)
      lons = np.linspace(lon1, lon2, num=numsites, endpoint=True)
      ids = np.linspace(1,numsites,num=numsites,endpoint=True)
     ```
   * method 3: pulling coordinates for polygon coordinates, explanation of polygon coordinates can be found in <**insert file name**>
   * note: There are many methods for pulling point or polygon coordinates, feel free to use the method that works best for you. 
5. Make a feature list out of your site data

```
# we will then create a google earth engine "feature collection" using this information
fts_list = []
for i in range(len(ids)):
  ft = ee.Feature(ee.Geometry.Point((lons[i],lats[i])))
  ft = ft.set({'Site':str(ids[i])})
  fts_list.append(ft)
sites = ee.FeatureCollection(fts_list)

# function for extracting value from a raster via GEE, we will use it momentarily
def extract(image):
  val = image.reduceRegions(sites, ee.Reducer.mean())
  return val
```

6. Extract monthly, daily, or hourly data for sites


# Structure of the repository
   * see ___ for monthly code
   * see ____ for daily and hourly code
   * notebooks include how to extract elevation data for each point using the USGS / SRTM digital elevation model (DEM) and how to display an image of data using ee.ImageCollection option
   * ____ also includes instructions for extracting polygone data
