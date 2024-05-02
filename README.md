
https://github.com/domlysz/BlenderGIS  

- github: on the right, there is releases...
- you can/should download master (most updated) (./BlenderGIS-master.zip)
- latest release v2.2.8 -> download source code (./BlenderGIS-228.zip)
- open blender -> edit -> preferences -> add-ons -> install -> point to the downloaded zip to install BlenderGIS add-on
- once installed -> (view/select/add/object/GIS) notice GIS menu button

#### what BlenderGIS does
- select any portion of the map
- generate a texture using: googles satalite imagery 
- generate a height map using: NASA's elevation data provided online to create 3d terrain meshes inside blender
 
#### google maps directly in blender
- GIS -> web geodata -> base map
- G (search for area)
- when area decided.. press "e" (plugin gives error but thats fine)
- material preview mode: you now have a flat cutout of your area
- menu (n) -> view clip start 0.001 and end 50000m (so nothing gets clipped off)

#### get elevation
- GIS -> web geodata -> get elevation (SRTM)
- server: OpenTopography 30m (sign up for api key: opentopography.org)
- once elevation is imported, blender adds this as a Texture Displacement modifier to a plane
- add subdivision surface modifier (if it is not already added..add it -> move it before the displace modifier): set levels viewport to 4 
- by default the elevation data is accurate (realistic) by you can change z-scale in object menu (right panel)
- materials -> roughness: 1.0
- add light (sun: strength 10) OR even better instead of adding a light: shader type: world (shading viewport) add node (search "sky texture")
- OR add light -> add hdri to world color (see "world" below)
- you can mute a node in shading viewport (M)

![GIS-CapeTown-render](./GIS-cape-town-render.png)

#### camera view
- sometimes camera dissapears
- reset (alt G , alt R) 
- apply all transformations
- camera object panel -> ensure z is above the map Grab (g + z -> 1000m)
- camera  object data properties -> fix  clip start(0.01) / end (50000m)

#### make it look better /realistic
- layout workspace -> render properties (right panel) -> color management -> view transform -> filmic
- layout workspace -> render properties (right panel) -> color management -> look -> very high contrast
- scale down (select all) -> to about 0.2
- object mode with terrain selected -> add bump node 
- and connect color data of exported google map with bump node "height" (strength 3.0, distance 3.0)
- connect bump normal to principle bsdf normal

#### model-side earth depth
- here you want to apply your modifiers to give model shape 
- first apply subdivision surface modifier
- then apply displacement (elevation auto-added modifier) modifier
- edit mode -> select all -> extrude down on z (depth)
- then scale on z (S + Z + 0)
- edit mode -> faces mode -> side view (3) -> select edge loop (select face, then alt select an edge) -> assign material
- edit mode -> select side faces -> mesh -> normals -> flip
- ensure normals are correct (blue) 
- material -> backface culling (side sided faces)

#### shading workspace -> object mode -> google imported model selected
- layout workspace -> render properties (right panel) -> film -> transparent (use light from hdri but dont show)
- add ambient occlusion node -> distance set high (25)
- add converter -> math node -> set to power (25)
- add color -> color MixRGB -> add it between google imported and principle BSDF -> change mix to "multiply"
- then connect power value to the mixRGB color B.
- principle BSDF -> specular -> 0.1

#### World
- world settings (right panel) -> color (round button on right of color) -> open hdri "environment texture" (download from polyhaven.com)
- the hdri only render in cycles

#### TODO:
- add a cube and make it look like seawater (settings: roughness 0.1, transmission weight 1, blueish color, principled volume with very low density 0.0002)
- add buildings -> GIS -> web geodata -> get Open street map data

#### References
- ClickbaitScience - https://www.youtube.com/watch?v=xrh4apJKhrs&ab_channel=ClickbaitScience
- CGGeek - https://www.youtube.com/watch?v=Mj7Z1P2hUWk&ab_channel=CGGeek
