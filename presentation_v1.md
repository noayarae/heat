#Heatmap
Spring term | Geovisual Analytics 572

By **Efrain Noa-Yarasca**

## 1. Introdution
Heatmaps are graphical representation of data where each determined zone or area (county, state, pixel) is shaded in proportion to its data.

They can be used to represent population density, diversity, average temperatures, per-capita income, social views, and many other geographically important measurements. For example:[population growth map]

[population growth map]:http://www.creativeclass.com/_v3/creative_class/_wordpress/wp-content/uploads/2011/04/Untitled.png

<center><img src="http://www.creativeclass.com/_v3/creative_class/_wordpress/wp-content/uploads/2011/04/Untitled.png" width="400"></center>

If the information is given in pixels or cells, a “smooth” heatmap can be obtained like [annual average precipitation map]

[annual average precipitation map]: http://climate.ncsu.edu/secc_edu/images/us_precip_average_1961_1990.gif

![annual average precipitation map](http://climate.ncsu.edu/secc_edu/images/us_precip_average_1961_1990.gif)

### Colors

[Colors] in a heatmap usually go from warm colors such as <span style="color:red">***red***</span>, <span style="color:orange"> ***orange*** </span> to cool colors such as <span style="color:blue">***blue***</span>, and <span style="color:green"> ***green*** </span>.

[Colors]:http://www.maxmayo.com/wp-content/uploads/2013/03/warm-cool-color-wheel-580x541.jpg

[comment: other way to get figures]:![Colors](http://www.maxmayo.com/wp-content/uploads/2013/03/warm-cool-color-wheel-580x541.jpg)

<center><img src="http://www.maxmayo.com/wp-content/uploads/2013/03/warm-cool-color-wheel-580x541.jpg" width="300"></center>

###Practices for designing heatmaps

 Tips to obtain a [nice heatmap] are summarized in four points:

- Use a simple scale of colors.
- Select colors appropriately.
- Use patterns sparingly.
- Choose appropriate data ranges.

[nice heatmap]: https://visage.co/data-visualization-101-heat-maps/

## 2. Heatmap design
Heatmaps can be designed on several servers such as [Geoserver], leaflet.

[Geoserver]: http://docs.geoserver.org/

###2.1 Making heat map by Geoserver

####Rendering transformatons

Rendering Transformations allow processing to be carried out on datasets within the GeoServer rendering pipeline. A typical transformation computes a derived or aggregated result from the input data, allowing various useful visualization effects to be obtained. Transformations may transform data from one format into another (i.e vector to raster or vice-versa), to provide an appropriate format for display

#### Usage

Rendering Transformations are invoked by adding the  element to a  element in an SLD document. This element specifies the name of the transformation process, and usually includes parameter values controlling the operation of the transformation [read more].

#### Heatmap generation

***gs:Heatmap*** is a Vector-to-Raster rendering transformation which generates a heatmap surface from weighted point data. The following SLD invokes a Heatmap rendering transformation on a featuretype with point geometries. The output is styled using a color ramp across the output data value range 0 to 1, [read more].

[read more]:http://docs.geoserver.org/stable/en/user/styling/sld/extensions/rendering-transform.html

Key aspects of the SLD are:

- Lines 14-15 define the rendering transformation, using the process vec:Heatmap.
- Lines 16-18 supply the input data parameter, named data in this process.
- Lines 19-22 supply a value for the process’s weightAttr parameter, which specifies the input attribute providing a weight for each data point.
- Lines 23-29 supply the value for the radiusPixels parameter, which controls the “spread” of the heatmap around each point. In this SLD the value of this parameter may be supplied by a SLD substitution variable called radius, with a default value of 100 pixels.
- Lines 30-33 supply the pixelsPerCell parameter, which controls the resolution at which the heatmap raster is computed.
- Lines 34-38 supply the outputBBOX parameter, which is given the value of the standard SLD environment variable wms_bbox.
- Lines 40-45 supply the outputWidth parameter, which is given the value of the standard SLD environment variable wms_width.
- Lines 46-52 supply the outputHeight parameter, which is given the value of the standard SLD environment variable wms_height.
- Lines 55-70 specify a RasterSymbolizer to style the computed raster surface. The symbolizer contains a ramped color map for the data range [0..1].
- Line 58 specifies the geometry attribute of the input featuretype, which is necessary to pass SLD validation.

This transformation styles a layer to produce a heatmap surface for the data in the requested map extent, as shown in the image below. (The map image also shows the original input data points styled by another SLD, as well as a base map layer.)

<center><img src="http://docs.geoserver.org/stable/en/user/_images/heatmap_urban_us_east.png" width="400"></center>

###2.1 Making heat map by using leaflet

Constructs a heatmap layer given an array of points and an object with the following options:

- **minOpacity** - the minimum opacity the heat will start at
- **maxZoom** - zoom level where the points reach maximum intensity (as intensity scales with zoom),
equals maxZoom of the map by default
- **max** - maximum point intensity, 1.0 by default
- **radius** - radius of each "point" of the heatmap, 25 by default
- **blur** - amount of blur, 15 by default
- **gradient** - color gradient config, e.g. {0.4: 'blue', 0.65: 'lime', 1: 'red'}

Each point in the input array can be either an array like [50.5, 30.5, 0.5],
or a [Leaflet LatLng object].

[Leaflet LatLng object]:http://leafletjs.com/reference-1.0.3.html

Optional third argument in each LatLng point (altitude) represents point intensity.
Unless max option is specified, intensity should range between 0.0 and 1.0.

#### Methods

- **setOptions(options)**: Sets new heatmap options and redraws it.
- **addLatLng(latlng)**: Adds a new point to the heatmap and redraws it.
- **setLatLngs(latlngs)**: Resets heatmap data and redraws it.
- **redraw()**: Redraws the heatmap.

Using this Heatmap plugin for leaflet, I created a heatmap of cell towers in Oregon. Please pay attention to how to link in the **leaflet heat plugin**, and how to use the **heatlayer**.

<center><img src="http://docs.geoserver.org/stable/en/user/_images/heatmap_urban_us_east.png" width="400"></center>


