
The goal of this tool is to provide an interface for auto-generating naturalistic RGB color palettes 
that can be copy-pasted into Battlefield 2 sky.con light settings.


Battlefield 2 rendering logic: 

Color brightness adjusts light strength. Setting a light to 0/0/0 turns it off, 1/1/1 is perfect white.

Direct light colors and indirect shadow colors are blended *additively*, i.e. mixing RGB 1/0/0 and 0/1/0 will create yellow.

Download the latest .html and open it in your browser.

---------------------------------------------------------------
Controllers 
---------------------------------------------------------------

Four recommended steps to start with

1.) Map base color
--------------

This is the ground reflected by the foliage. The idea is that at full sunlight the ground reflects on 
the foliage green. So on i.e. Karkand instead of a lush green it would make sense to pick a sand tone.

## -> Influence on foliage

	Automatically adjusts the ground color influence on foliage with sun intensity. 
	This slider can be manually overridden.


2.) Get Sun angle
-------------
In the sky.con the parameter Lightmanager.sunDirection XXX/YYY/ZZZ shows you a given sun direction.
Input the YYY value into the box to get a readable value in degrees. 

Has no effect on the model but acts as a simple translator.

3.) Time of day
--------------
Look at the sky box texture, what time of day makes sense here? If it's morning on Dalian Plant then you may dial
the time-of-day slider to 24° at around 7 AM.

4.) Cloud cover
---------------
Very decisive effect for your map. Shifts the whole illumination from direct to indirect light, and greys out the whole scene. 
Slider has to be manually set.

Ground elevation
--------------
Climbing up high means less atmosphere above you, so the sun becomes brighter and shadows bluer.
This makes for a distinct look for maps like i.e. Fushe Pass.

Sun Position
------------
Where the sun currently sits relative to the observer. At 0° it's at the horizon at dawn or dusk and gives off warm light. 
At 90° it's directly above your head at noon, and gives off bright white-ish light.

Sun intensity
------------
The sun is not always shining the same strength. This slider can be manually adjusted.

Air type
------------
The type of stuff that's moving through the air giving indirect light it's distinct color.
Especially on Highway Tampa or Karkand you may not want a blue sky coloring for your shadows but give the map a more dusty brown-ish look.

## ->  influences __Haze (turbidity)__

	The amount of stuff that's moving through the air. Turning it to 0.0 restores a blue sky.

## -> influences __twilight tint (high altitude turbidity)__
	
	The same thing with different effect. Why is the sky sometimes pink or purple at sunset or dawn? 
	The answer is stuff is moving through the air at high altitudes. 
	Effect will be visible at 20° degrees or below.

	Try out the smoke/ volcano air type at dawn or dusk to see a striking difference.
	Only affects shadows at daytime. Affects direct and indirect light at night.


--------------------------------------------------
Advanced:
--------------------------------------------------


Shadow compensator (auto) 
------------
This is a gate that prevents the shadows from becoming too black. Because Battlefield 2 has no secondary reflections this parameter 
automatically shifts the light budget from direct light to the indirect light while preserving it's color.


StaticMesh brightness/ Terrain brightness
------------
Manual parameters to play with. Preserves color.


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






