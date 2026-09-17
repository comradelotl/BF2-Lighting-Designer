
The goal of this tool is to provide an interface for auto-generating naturalistic RGB color palettes 
that can be copy-pasted into Battlefield 2 map light settings in the sky.con file.


Battlefield 2 rendering logic: 
Color brightness adjusts light strength. Setting a light to 0/0/0 turns it off.
Direct light colors and indirect shadow colors are blended *additively*, i.e. mixing RGB 1/0/0 and 0/1/0 will create yellow.
 

---------------------------------------------------------------
Controllers 
---------------------------------------------------------------

Four recommended steps to start with

1.) Map base color
--------------

This is the ground reflected by the foliage shadow. The idea is that at full sunlight the ground reflects on 
the foliage green. So on i.e. Karkand instead of a lush green it would make sense to pick a sand tone.

## -> Influence on foliage

	Automatically adjusts the ground color influence on foliage with sun intensity. This slider can be manually adjusted.


2.) Get Sun angle
-------------
In the sky.con the parameter Lightmanager.sunDirection XXX/YYY/ZZZ shows you a given sun direction.
Input the YYY value into the box to receive a readable value in degrees. 

3.) Time of day
--------------
Look at the sky box texture, what time of day makes here? If it's rather morning like in Dalian Plant then dialing 
the time of day slider to 24° at 7:36 makes the most sense. 

4.) Cloud cover
---------------
Very decisive effect for your map. Shifts the whole illumination from direct to indirect light and makes the light more grey. 
Slider has to be manually set.

Ground elevation
--------------
The sun will become brighter and the shadows bluer. 

Sun Position
------------
Where the sun currently sits relative to the observer. At 0° it's at the horizon at dawn or dusk and gives off warm light. 
At 90° it's directly above your head at noon,  is the brightest and gives off white-ish light.

Sun intensity
------------
The sun is not always shining the same strength. This slider can be manually adjusted.

Air type
------------
The type of stuff that's moving through the air and giving indirect light it's distinct color.
Especially on Highway Tampa or Karkand you may not want a blue sky coloring your map but give the map a more dusty brown-ish look.

## ->  influences __Haze (turbidity)__

	The amount of stuff that's moving through the air. Turning it to 0 restores a blue sky.

## -> influences __twilight tint (high altitude turbidity)__
	
	The same thing with a different effect. Why is the sky sometimes pink or red at sunset or dawn? 
	The answer is stuff is moving through the air at high altitudes. 
	Effect will be visible at 20° degrees or below.

	Try out the smoke/ volcano air type at dawn or dusk to see the difference.
	Also affects moonlight.


--------------------------------------------------
Advanced:
--------------------------------------------------


Shadow compensator (auto) 
------------
This is a gate that prevents the shadow from becoming too black. Because Battlefield 2 has no secondary reflections this parameter 
automatically shifts light budget from direct light to the indirect light.


StaticMesh brightness/ Terrain brightness
------------
Manual parameters to play with. Preserves hue.


----------------------------------------------------------------------------
Output
----------------------------------------------------------------------------

Material type						| Config Variable
------------------------------------| -------------------------------------------------------------------------
Vehicles, Weapons and Soldiers		| Lightmanager.sunColor, Lightmanager.skycolor, Lightmanager.sunSpecColor
Terrain and cast shadows 			| terrain.sunColor, terrain.GIColor
Buildings, objects and tree trunks 	| Lightmanager.staticSunColor, Lightmanager.staticSkyColor, Lightmanager.staticSpecularColor
Tree foliage, shrub and grass		| Lightmanager.treeSunColor, Lightmanager.treeAmbientColor
Smoke, fire, water effects, etc.	| Lightmanager.effectSunColor, Lightmanager.effectShadowColor
------------------------------------| -------------------------------------------------------------------------
Dummy values left for copy-pasting	| Lightmanager.ambientcolor, Lightmanager.treeSkyColor
that are not used ingame
------------------------------------| -------------------------------------------------------------------------
Experimental, may be better hand picked	| Renderer.fogColor






