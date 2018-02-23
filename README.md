# SpatialFunction4FisheryEcologyAnalysis
Some helpful function to use in fishery spatial analysis.

##dist2port
Estimate distance of a point to the nearest port and, if using <a href="http://www.naturalearthdata.com/downloads/10m-cultural-vectors/ports/"> Natual Earth shapefile of world's ports </a>, retrieves name and coordinates of the ports.

```{r global_options, include = FALSE}
dist2port = function(pp, ports){
  require(sp)
  distport = NULL
  wgs84 = "+proj=longlat +ellps=WGS84 +datum=WGS84 +no_defs"
  for(i in 1:nrow(pp)){
  xpp = SpatialPoints(pp[i,], proj4string = crs(wgs84))
  distmat = spDists(ports@coords, xpp@coords, longlat = T)
  dist_ok = distmat[which.min(distmat),]
  port_name = ports@data[which.min(distmat),"name"]
  port_coord = ports@coords[which.min(distmat),]
  distport = rbind(distport, data.frame(name = port_name, x = port_coord[1], y = port_coord[2], dist = dist_ok))
  rownames(distport)[i] <- i
  }
  return(distport)
}
```
