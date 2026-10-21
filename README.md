![alt text](https://github.com/comradelotl/BF2-Lighting-Designer/blob/main/screen249.png)

![alt text](https://github.com/comradelotl/BF2-Lighting-Designer/blob/main/screen254.png)

10/20/26
update coming soon

![alt text](https://t3.ftcdn.net/jpg/00/51/45/62/240_F_51456278_3y8SC99iozvWZZXfUdtJ21TlJaqgQHWK.jpg)

--------------

The Lighting Designer provides an interface for auto-generating naturalistic RGB color palettes.

The Fog & Water Designer provides visual reference for more intentional color design.

For daytime this is actually a physics engine in disguise. It's not simply calculating sun beam and sky dome colors, but all four types of mesh are given their own kind of filter to model material surface response to light. Read the technical notes below.

Bug report: contact me via ModDB or Discord

Contains 45 skybox textures, hence the ~10 MB


First steps:
---------------

Download the latest .html and open it in your browser.

For Battlefield 2: drop the levels/[name]/server.zip and start dialing. The html will read out your sky.con automatically. 

For Battlefield 2142: I didn't get my hands on skybox textures yet, but you can upload them yourself.


server.zip drop here
---------------

automatically reads the sky.con,
selects the sky texture from the archive and
enters the sun angle value


Battlefield 2 rendering logic: 
---------------

RGB channel intensity represents the light intensity. 
Setting a light to 0/0/0 turns it off, 1/1/1 is perfect white. The brightness can exceed 1 because BF2 rendering accepts HDR.
Direct light colors and indirect shadow colors are blended *additively*, i.e. mixing RGB 1/0/0 and 0/1/0 will create yellow.
Effects are the odd exception to this. Tuning effectsSun 0/0/0 turns them black.

------------------------------
Lighting Designer Controllers 
------------------------------


1.) Foliage Ambient Color 
--------------
Give foliage a custom tint around noon.
Foliage will always stay green regardless.
This is mainly for giving the option to create some visual coherency. 


2.) Ground elevation
--------------
Reduces the influence of haze and brightens the scene.
	
		The amount of air above the scene
	
3.) Haze (turbidity)
--------------

Reddens direct light and tints diffuse light. Turning it to 0.0 restores a blue sky.
Deactivates fogginess

		The amount of stuff that's moving through the air. 
		This is the answer to the question: why are desert sunsets so orange?
		Is lowest in the morning and rises towards the sunset.
		Naturally reduces with ground elevation.

-> determines __twilight tint (high altitude turbidity)__

5.) Twilight tint
--------------
Similar cause with different effect. 

		Why is the sky sometimes pink or purple at sunset or dawn? 
		The answer is stuff is moving through the air at high altitudes. 
		Effect will be visible at 20° degrees and below.

4.) Fogginess
--------------
Washes out the blue diffuse light of the "humid / maritime" option exclusively. 
Is highest in the morning and evening, lowest at noon.
Deactivates haze.
	
5.) Cloud coverage
--------------
How covered the sky is in eights.
Shifts the amount of light from direct towards diffuse light. Reduces shadows and desaturates colors.

Two options.
- Thin clouds leave 61 % of total light under full overcast. 
- Thick clouds leave 25 % of total light under full overcast.

		Cloud coverage measured in okta. Overcast gives the scene a slightly blue tint. 
		Direct light stays saturated while being reduced.

6.) Material reflectance
--------------
My model has inbuilt material filters for the different types of mesh. 
Disabeling it unlocks the direct raw sun beam and sky dome. This gives a more washed out look for the scene.


7.) Brightness adjustment
--------------
Output brightness control. 

Can for example be used to brighten shadows of a snowy scenes


Advanced: Shadow compensator (auto) 
------------
This is a gate that prevents the shadows from blacking out. Because Battlefield 2 has no secondary reflections this parameter 
automatically shifts an amount of light  from direct light to the indirect light.

treeAmbientColor is excluded from this.


----------------------------------------------------------------------------
Output. 0-1 RGB (HDR)
----------------------------------------------------------------------------

All four main Mesh types have a filter applied to give them a stable identity under shifting lighting conditions.
So i.e. buildings under diffuse light don't go completely blue.

Note: StaticSunColor is intentionally given a reduced brightness, that's there to compensate a Battlefield 2 engine quirk.

Material type							| Config Variable
----------------------------------------| -------------------------------------------------------------------------
1.) Vehicles, Weapons and Soldiers		| Lightmanager.sunColor, Lightmanager.skycolor, Lightmanager.sunSpecColor
2.) Terrain and cast shadows 			| terrain.sunColor, terrain.GIColor
3.) Buildings, objects and tree trunks 	| Lightmanager.staticSunColor, Lightmanager.staticSkyColor, Lightmanager.staticSpecularColor
4.) Tree foliage, shrub and grass		| Lightmanager.treeSunColor, Lightmanager.treeAmbientColor
Smoke, fire, water effects, etc.		| Lightmanager.effectSunColor, Lightmanager.effectShadowColor
Dummy values left for copy-pasting that are not used ingame | Lightmanager.ambientcolor, Lightmanager.treeSkyColor


----------------------------------------------
########### Fog & Water Designer #############
----------------------------------------------

This is a logical extension of the lighting designer.

currently contains Battlefield 2 skybox textures only.


Skybox texture management: 
------------------------

Adding new skybox textures: 
1.) drop them directly into the browser
2.) save into html: downloads a copy containing the new texture

Removing textures:
1.) enable editing and remove existing textures
2.) save into html

Fog color: 0-255 RGB. Sky.con: renderer.fogColor
----------------

Pick a fog color from the skybox texture directly.


Water body: 0-1 RGB. Water.con: renderer.waterColor
----------------
Derive a water color from the fog color. Fog connects the sky-box texture and the water plane.

Controls are in LCh and can be overriden
- Lightness: Brightness (relative, -50% the fog color at preset)
- Chroma: Colorfulness (relative, -50% the fog color at preset). 
- hue: tracks fog value at preset. 

Sunglint: 0-1 RGB. Water.con: renderer.waterSpecularColor
-----------------
Only visible in water when you face the sun direction.

This is the color of the direct sun beam, carried over from the lighting designer. 
Hence at night without moon or at overcast there will be no sunglint

copy and paste into the first three characters R/G/B/XXX

 

Limitations:
---------
The water body color is sometimes like in Karkand also determined by the groundhemi.dds.
So if you want to remove this or change this you have to edit this texture directly.

Battlefield 2 Water render contains a scrolling normal map, which is not to be confused with the sunglint.
It's contribution to color can not be edited. 
Also the water render prevents water from becoming completely black 0/0/0. 
The preview patch tries to simulate this.

Technical notes:
----------------

All daytime color values are calculated using atmospheric physics.
I merely authored the color palettes for twilight and night.

Colors are calculated and processed using XYZ and OKLCh color spaces. Color spaces are assigned specific tasks to achieve specific goals. 
The output display is just severely compressed to simple RGB triplets for BF2. So this model could easily be adapted to other types of game engines. 

The rough model pipeline construction is:

1.) Input: physics calculation layer for deriving sun beam, sky dome, turbidity (CAMS aerosol properties) and fog values.

2.) a light budget & distribution system for the direct/ diffuse relation according to ASHRAE & Perez, etc. 
The different color spaces allow me to completely control the amount of light

3.) Material_response: All different colors emitted by sun beam and sky dome are put through color filters to give statics, dynamic objects, terrain and especially foliage their specific identity and not have them be simple passive reflectors.

4.) Output: lightness adjustments for game design purposes.

Rendering quirk:
I have discovered that lighting all four types of mesh the same amount of light leaves statics overexposed.
So what one may perceive in the staticSunColor as rather dim is the fair share that gives directly lit
static surfaces an appropriately balanced look. 





------------------

Sources: 

Atmospheric physics:

https://github.com/elliahu/atmosphere 

https://ebruneton.github.io/precomputed_atmospheric_scattering/

Material filtering

https://github.com/colour-science/MunsellAndKubelkaMunkToolbox

https://merl.com/research/downloads/BRDF 

Aerosols:

https://github.com/cosmoscout/cosmoscout-vr-clouds/tree/main/plugins/csp-atmospheres

https://github.com/CriticalAssetManagement 

---------

Arranged with Claude & OpenCode
