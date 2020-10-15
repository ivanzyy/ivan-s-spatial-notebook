# ivan-s-spatial-notebook
anyone can use this to learn
library(maptools)
library(sp)
library(spdep)
library(tripack)
hebeimap <- readShapePoly("D:/application/rstudy/hebei1.shp")
plot(hebeimap)
names(hebeimap)
head(hebeimap)
proj4string(hebeimap) <- "+proj=longlat + datum = WGS84"
spplot(hebeimap[c("tfp","tp")],
       main = "spatial distribution of 93",
       xlab = "x coords",
       ylab = "y coords",
       cut = 30)
spplot(hebeimap["tfp"],
       main = "spatial distribution of 93",
       xlab = "x coords",
       ylab = "y coords",
       cut = 30)
queen_nb <- poly2nb(hebeimap,queen = T)
rook_nb <- poly2nb(hebeimap,queen = F)
coords <- coordinates(hebeimap)
IDs <- row.names(as.data.frame(hebeimap))
opar <- par(no.readonly = T)
oopar <- par(mfrow = c(1,2),mar = c(3,3,1,1) + 0.1)
plot(hebeimap,border = "grey",main = "queen style")
plot(hebeimap,coords,col = "dodgerblue",add = T,pch = 19,cex = 0.5)
k4_nb <- knn2nb(knearneigh(coord,k = 6),row.names = IDs)
is.symmetric.nb(k4_nb,verbose = F,force = T)
n.comp.nb(k4_nb)$nc
k4_w <- nb2listw(k4_nb)
moran.test(hebeimap$tfp,listw = k4_w)
moran.mc(hebeimap$tfp,listw = k4_w,nsim = 999)
