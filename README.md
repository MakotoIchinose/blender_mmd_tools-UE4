mmd_tools
===========

Overview
----
mmd_tools is a blender addon for importing MMD (MikuMikuDance) model data (.pmd, .pmx), motion data (.vmd) and pose data (.vpd). Exporting model data (.pmx), motion data (.vmd) and pose data (.vpd) are supported as well. This particular branch is modified to follow Unreal Engine 4's naming convention for armature, in the hope of easing the process of retargeting animations.

### Environment

#### Compatible Versions
 - Blender 2.80 or later

Usage
---------

### Precautions for retargeting
Please note that MMD armatures does not universally adhere to game engine armature standard. This can cause problems when retargeting animation montages/actions and scripts that was made for UE4 armature in mind to your desired MMD model.

1. Most MMD armatures only have 2 spine bones, as opposed to Unreal Engine's 3 spine bone standard. This makes retargeting UE animations a bit confusing. In that case, you can skip UE's *spine_01* bone when retargeting animations.
2. Most, *but not all*, MMD armatures also following their own MMD bone parenting convention, which by itself, does not adhere to game engine armature standard. This is a common problem with MMD models, and unfortunately, there is no "one solution fits all" for this case. However, you can adapt the bone parenting of the model you use by yourself to fit the game engine standard.

### Install
Simply install the zip you downloaded from the GitHub page through Blender's Add-on Manager in Preferences. No manual extraction needed.

### Loading Addon
1. User Preferences -> addon tab -> select the User filter with Community in 'Supported Level' check, then click the checkbox before mmd_tools (you can also find the addon using the search function)
2. After the installation, you can find two panels called mmd_tools and mmd_utils on the left of the 3D view

### Configuring Addon
The following settings can be configured under mmd_tools Addon Preferences.
* Shared Toon Texture Folder
    * The `Data` directory within of your MikuMikuDance directory for toon textures (_toon01.bmp ~ toon10.bmp_).
* Base Texture Folder
    * Path for textures shared between models. In order to copy textures properly while exporting to pmx file.
* Dictionary Folder
    * Path for searching custom csv dictionaries. This particular branch is following Unreal Engine's bone naming convention.

### Importing a model
1. Go to the mmd_tools panel
2. Press _Import Model_ and select a .pmx or .pmd file


### Importing motion data
1. Load the MMD model. Then, select the imported model's Mesh, Armature and Camera.
2. Click the _Import Motion_ button of the mmd_tools panel.
3. If a rigid body simulation is needed, press the _Build_ button in the same panel.

Turn on the "update scene settings" checkbox to automatically update scene settings such as frame range after the motion import.

### Exporting a model to FBX

Due to Blender's exporting behaviour on skeletal meshes, and Unreal Engine's restriction of **one** root bone, 



Various functions in detail
-------------------------------
### Import Model
Imports a MMD model. Corresponding file formats are .pmd and .pmx (version 2.0).

* Scale
    * Size of the model
* Rename bones
    * Renames the bones to be more blender suitable
* Use MIP map for UV textures
    * Specify if mipmaps will be generated
    * Please turn it off when the textures seem messed up
* Influence of .sph textures and influence of .spa textures
    * Used to set the diffuse_color_factor of texture slot for sphere textures.

### Import Motion
Apply motion imported from vmd file to the Armature, Mesh and Camera currently selected.
* Scale
    * Recommended to make it the same as the imported model's scale
* Margin
    * It is used to add/offset some frames before the motion start, so the rigid bodies can have more smooth transition at the beginning
* Update scene settings
    * Performs automatic setting of the camera range and frame rate after the motion data read.


Notes
------
* If camera and character motions are not in one file, import two times. First, select Armature and Mesh and import the character motion. Second, select Camera and import the camera motion.
* When mmd_tool imports motion data, mmd_tool uses the bone name to apply motion to each bone.
    * If the bone name and the bones structure matches with MMD model, you can import the motion to any model.
    * When you import MMD model by using other than mmd_tools, match bone names to those in MMD model.
* mmd_tools creates an empty model named "MMD_Camera" and assigns the motion to the object.
* If you want to import multiple motions or want to import motion with offset, edit the motion with NLA editor.
* If the origin point of the motion is significantly different from the origin point of the model, rigid body simulation might break. If this happens, increase "margin" parameter in vmd import.
* mmd_tool adds blank frames to avoid the glitch from physics simulation. The number of the blank frames is "margin" parameter in vmd import.
    * The imported motion starts from the frame number "margin" + 1. (e.g. If margin is 5, frame 6 is frame 0 in vmd motion)
* pmx import
    * If the weight information is SDEF, mmd_tool processes as if it is in BDEF2.
    * mmd_tool only supports vertex morph.
    * mmd_tool treats "Physics + Bone location match" in rigid body setting as "Physics simulation".
* Use the same scale in case you import multiple pmx files.


More Informations
----------
* See https://github.com/powroupi/blender_mmd_tools/wiki


Known issues
----------
* See https://github.com/powroupi/blender_mmd_tools/issues


License
----------
Distributed under the GPLv3 License.
