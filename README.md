# R_SpatialFunction4FisheryEcologyAnalysis
Some helpful function to use in fishery spatial analysis.

##dist2port

Estimate distance (in Km) of a point to the nearest harbour. This function retrieves name and coordinates of the neareset harbour from the shapefile of world's ports from <a href="http://www.naturalearthdata.com/downloads/10m-cultural-vectors/ports/"> Natual Earth </a> web site.

```{r global_options, include = FALSE}
##Inputs
#pp: 2 column data.frame (x,y) in wgs84
#ports: Shapefile of world's port from Natural Earth web site
##Output
#distport = dataframe with name, coords and distance (km) 

dist2port = function(pp, ports){
  require(sp)
  distport = NULL
  wgs84 = "+proj=longlat +ellps=WGS84 +datum=WGS84 +no_defs"
  for(i in 1:nrow(pp)){
  xpp = SpatialPoints(pp[i,], proj4string = crs(wgs84))
  distmat = spDists(ports@coords, xpp@coords, longlat = T)
  dist = distmat[which.min(distmat),]
  port_name = ports@data[which.min(distmat),"name"]
  port_coord = ports@coords[which.min(distmat),]
  distport = rbind(distport, data.frame(port_name = port_name, port_lon = as.numeric(port_coord[1]), port_lat = as.numeric(port_coord[2]), dist_port = dist))
  }
  return(distport)
}
```
