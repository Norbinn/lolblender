# Requirements #
This is an addon for the Blender3D 2.5 branch.  It has been tested to work
on 2.54

Blender homepage:  http://www.blender.org

Daily builds:      http://www.graphicall.org

# Installing #
To install, copy the io\_scene\_lol folder and contents
to the scripts/addons directory of your blender install.
```
blender/2.54/scripts/addons
```

## USING ##
These scripts can be used two ways, through the blender UI and from a python console within blender.
### As a Blender Plugin ###
The prefered way to use these scripts is through Blender's own UI and the plugin interface.  Open up Blender's user preferences
![http://imgur.com/b8Wv4.png](http://imgur.com/b8Wv4.png)

and select 'Addons', then 'Import/Export'.  Find the Leauge of Legends addon and click the small box to the right to enable it.  If you want this addon to load upon starting Blender, click 'Save as Default', otherwise just close the window.
![http://imgur.com/D0bhy.png](http://imgur.com/D0bhy.png)

Now you have import/export options under the File menu

![http://imgur.com/aUbAq.png](http://imgur.com/aUbAq.png)

To import a character mesh into blender, choose File>import>League of Legends and navigate to the directory containing your desired character (Now would be a good time to make a backup of the mesh you'll overwrite eventually).  Select the mesh file (.skn), skeleton file (.skl) and optionally the skin texture (.dds).  The files that will be imported are listed on the left.  If you mistakenly chose the wrong file, just click the appropriate one before clicking 'Import'
![http://imgur.com/LsNl0.png](http://imgur.com/LsNl0.png)
### From python console within blender ###
Access a python console from within blender
![http://imgur.com/ZFqo4.png](http://imgur.com/ZFqo4.png)

After placing the io\_scene\_lol folder into the blender addons directory the
scripts will be within blender's python path.

'>>' denotes the python console prompt.

import the plugin scripts:
```
>>import io_scene_lol
```
Say you wanted to import Gankplank.  Gankplank is the 'Pirate'
model in the `HeroPak_client` directory.  The default Gankplank
model has three base components:

  * Pirate.skn:		The model mesh
  * Pirate.skl:		The model skeleton
  * Pirate.dds:		The model skin (texture)

To import this model use the import\_char() function.  First
set up some convenience variables:
```
>>base = 'c:\\path\\to\\DATA\\Characters\\Pirate'
>>skn  = 'Pirate.skn'
>>skl  = 'Pirate.skl'
>>tex  = 'Pirate.dds'

>>io_scene_lol.import_char(base, skn, skl, tex)
```
![http://imgur.com/423mx.png](http://imgur.com/423mx.png)

The model should now be imported.  import\_char has a few other
options too.  For more info type
```
>>help(io_scene_lol.import_char)
```
![http://imgur.com/4XE1v.png](http://imgur.com/4XE1v.png)

To export a character model use the export\_char(filename) command:
```
>>io_scene_lol.export_char(filename)
```
Here is some example output importing the Katarina mesh showing some mesh weighting:
![http://imgur.com/PQBe9.png](http://imgur.com/PQBe9.png)