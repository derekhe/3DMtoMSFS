# **Creating Custom Scenery in MSFS**
*subject to change*

### Many thanks to u/TheStoneFox for his incredible [YouTube tutorial](https://www.youtube.com/watch?v=ZdCP11rqpVk), from which all these notes derive

__TODO__ 
  * [ ] further Blender investigations around colour correction and texture 'blending'/merging into single texture file
  * [ ] possible 'setup script' that sorts out the first 'file management' aspects
  * [ ] further investigation into MSFS Dev tools to add lights etc within the Dev Editor
  * [ ] fall down a 3D modeling/scenery creation rabbithole


### Setup Project files
  * [ ] Install all pre-reqs as per notes on the [YouTube tutorial](https://www.youtube.com/watch?v=ZdCP11rqpVk) *(further notes to come)*
  * [ ] Copy ```SimpleScene``` from Sample SDK library into a 'projects area'
  * [ ] Rename copied sample folder to your project name, refered to as ```<project_folder>``` in this documentation
  * [ ] Rename ```<project_folder>\SceneryProject.xlm``` to ```<project_name>.xlm```
  * [ ] Rename folder ```<project_folder>\PackageDefinitions\mycompany-scene``` to ```<project_name>```
  * [ ] Rename ```<project_folder>\PackageDefinitions\mycompany-scene.xml``` to ```<project_name>.xml```
  * [ ] Delete all subfolders in ```<project_folder>\PackageSources\modelLib```
  * [ ] Create folder in ```<project_folder>\PackageSources\modelLib``` called **texture**
  * [ ] Create folder in ```<project_folder>\PackageSources\modelLib``` called **<project_name>Model**
  * [ ] Delete ```object.xml``` from ```<project_folder>\PackageSources\scene```
  * **Edit**  ```<project_folder>\<project_name>.xml```
    * [ ] Line 5 - replace ```'mycomapny-scene'``` with ```<project_name>```
  * **Edit** ```<project_folder>\PackageDefinitions\<project_name>.xml```
    * [ ] Line 1 - replace ```'mycomapny-scene'``` with ```<project_name>```
    * [ ] Line 16 - replace ```'mycompany-scene'``` with ```<project_name>```
    * [ ] line 17 - replace ```'mycompany-scene'``` with ```<project_name>```

### Run RenderDoc and Chrome in developer mode ###
  * [ ] Run RenderDoc
  * [ ] Open a CMD prompt and use the following two lines

*note your target for Chrome may be different - adjust accordingly*

    > set RENDERDOC_HOOK_EGL=0
    > "C:\Program Files\Google\Chrome\Application\chrome.exe" --disable-gpu-sandbox --gpu-startup-dialog

  * [ ] Make a note of the PID but don't 'OK' dialog box
  * [ ] In RenderDoc, go to ```File``` -> ```Inject into Process```
  * [ ] Filter by the Chrome PID and click ```Inject```
  * [ ] Ensure ```Connection Status``` shows ```Established```
  * [ ] Go back to Chrome and click 'OK' in the PID dialog
  * [ ] Go find scenery in prefered Photogrammetry viewer and consider hiding any text/labels
  * [ ] Once found and in 3D, in RenderDoc, click ```Capture Frame(s) Immediately```
  * [ ] **Return to Google Chrome and 'wiggle' the view a bit in the map to ensure good capture of data in RenderDoc**
  * [ ] Double click on the capture thumbnail and ensure you see at least 3 color passs in the Event Broswer, but no more than 4
  * [ ] ```File``` -> ```Save Capture As``` -> give filename and save somehwere sensible, but not in scenery project folder area
  * [ ] Close RenderDoc

### Bring into Blender and tidy up
  * [ ] Run Blender
  * [ ] Delete the default cube and camera
  * [ ] ```File``` -> ```Import``` -> ```Google Maps Capture```
  * [ ] Select the RDC file from Renderdoc
  * [ ] User Blender to clean up and trim down model to required size
    * **Tips**
    *  Go into 'X-Ray' mode.
    *  Select all by pressing 'a' and select ```join``` to bring all objects together.
    *  Hit TAB to enter edit mode and 'paint' the verticies you don't want and hit DELETE
  * [ ] Once ready, press 'a' followed by CTRL + J to 'join' all the textures together
  * [ ] Press 'a' to ensure whole model is selected and remains selected until you finish the export
  * [ ] ```File``` -> ```Export``` -> ```extended glTF 2.0 (.glb/.gltf) for MSFS```
  * [ ] Navigate to ```<project_folder>\PackageSources\modelLib\<Project_name>Model```
  * [ ] Provide filename ```<project_name>```
    **File Properties:**
    * Format      = ```glTF Seperate```
    * Textures    = ```..\texture\```
    * **MSFS** 
      * Generate/Append XML = Selected
      * XML Filename = ```<project_name>```
      * Generate GUID = Selected
    * **Include**
      * Selected Object = Selected
      * Custom Properties = Selected
    * **Geometry**
      * Apply Modifiers = Selected
    * **Leave all other options as default**
  * [ ] Click export (might take a few seconds)
  * [ ] Check texture folder for a bunch of images (no textures?  You probably didn't select the entire model)
  * [ ] Check ```<project_folder>\PackageSources\modelLib\<Project_name>Model``` folder for 3 files
  * Edit ```<project_folder>\PackageSources\modelLib\<Project_name>Model\<project_name>.xml```
    * [ ] Line 1 - change XML version number to '1.1' and add 'encoding="utf-8"

### Go into MSFS in Dev mode and load up the model etc
  * [ ] Get yourself onto the map near where you want your model to go
  * [ ] ```Options``` -> ```Pause Simulation```
  * [ ] ```Camera``` -> ```Developer Camera```
  * [ ] ```Tools``` -> ```Project Editor```
  * [ ] ```Project``` -> ```Open...```  
  * [ ] Open the project using the root <projecct_name>.xml created at the start of the Process
  * [ ] 'View' -> 'Inspector'
  * [ ] Fill out details of the project if you wish
  * [ ] Click 'Build Packages' (may take a few seconds)
  * [ ] Click 'myscene' (from Project Editor) and click 'Load in Editor' from the Inspector

### Postition and Save
  * [ ] Scenery Editor -> ```View``` -> ```Objects```
  * [ ] Select ```Polygon``` from dropdown and click 'Add'
  * [ ] Place exclusion Polygon around scenery you do not want any autogen scenery to show.
  * [ ] Right click on the centre point of the polygon and select 'Properties'. 
  * [ ] Select ```Exclude All```
  * [ ] Scenery Editor -> ```View``` -> ```Objects```
  * [ ] Select ```Scenery``` from dropdown
  * [ ] Search for your model and click 'Add'
  * [ ] Place model and scale/rotate etc using the 'Gizmo' tool
  * [ ] Highlight the top level project name in the Project Editor and in the Inspector, click ```Build Packages```
  * [ ] Once happy with placement and location, click ```Save Scnenry``` in the Scenery Editor
    * [ ] Save ```<project_name>SHP``` into ```<project_folder>\PackageSources\scene```
    * [ ] Save ```<project_name>SCN``` into ```<project_folder>\PackageSources\scene```

### Try it out
  * [ ] Move ```<Project_folder>\Package\<Project_Name>``` folder into community
  * [ ] Launch Game and go have a look...
Run MSFS
