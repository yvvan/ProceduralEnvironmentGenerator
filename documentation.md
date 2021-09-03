Basic workflow
-------------
You can either use Blueprints from the Demo content (WfcActorWithBlueprint, WfcUnlimited) or drop Wfc Actor to the Editor viewport and create a Blueprint for it.
To start setting up the rules for generation you need to first set up the Blueprint inside Blueprint Editor (Edit Blueprint button).

Bluepreint Editor workflow
-------------
Inside the Blueprint Editor you can set up the default settings for your Wfc Actor and add new components which are going to be the building blocks for the generation.
Press "Add Component -> Wfc Voxel" and set it's name to add the new building block.
For each component you've added you can specify the settings to each individual Wfc Voxel.
You can select components in the tree or by clicking on them in the viewport.

Wfc Actor:
-------------
For Wfc Actor the Details panel controls in Editor and in Blueprint Editor are almost identical.
Differences:
 - Blueprint Editor has "Import" and "Export" buttons to import and export the Blueprint settings from/to .json file
 - Editor has "Generate in Editor" and "Clean generated" buttons. "Generate in Editor" starts/restarts the generation process. "Clean generated" stops the generation or clears the editor from the earlier generated elements.
 - Editor has "Advanced" controls

Common controls:
-------------
 - Walkable Paths. You can add any number of paths containing 2 or more ends. The algorithm will make sure each path exists before completing the generation. The coordinates are set from the (0,0,0).
 - Components Max Count. Add components and their maximum count here if you want to limit how many blocks of this type you want to allow.
 - Fixed Components. This list contains the blocks which are fixed at the specified position (starting from (0,0,0)). It supports Ranges meaning that all the blocks in that range are going to be filled with the same component (with rotation). Setting negative numbers in some of
   the dimensions means that the numbering goes from the end of that dimension. For example "-1" is the last element, "-2" is the second one from the end and so on.
 - New Fixed Components. The same list as above but a temporary one. Press the button "Update Fixed" so that "New Fixed Components" are applied to the "Fixed Components".
 - Place separate Actors. When this checkbox is checked - every building block is spawned as and Actor. Otherwise it is spawned as Instanced Static Mesh.
 - X, Y, Z. Dimensions of the generation area.
 - Grows. Specifies if the level should be extended when you click at some actor in the level. Also this flag is required when you want to have a growing level in Runtime.
 - Clean Up After Growing. Is available only when "Grows" is on. Keeps the level maximum size specified by the "X", "Y", "Z" by removing everything outside of the updated generation area.
   This means for example that if you run in WfcUnlimited you get new level generated in front of you and the most distant parts behind are removed. This helps to avoid conflicts in the infinite generation.
 - Ground, Floor Border, Air Border, 2D Border, Render Borders. These settings are deprecated. Use "Fixed Components" instead.
 - Voxel Size. Size of the child Wfc Voxel components. By default this size is used for both x, y and z coordinates. It should match the real size of the components you set as children.
 - Voxel Height. Overrides the Z dimension of the Voxel Size.
 - Voxel Spacing. The additional space which is put in Blueprint Editor Viewport between each two voxels
 - Seed. The seed value for the random generator. If set to 0 - the value based on current time is used.
 - Last Seed. The seed of the last/current generation. You can copy it and use as Seed to get the same generation result.
 - Shadow Material. The material used to visualize neighbors in Blueprint Editor. There's the default one in the demo content with the name "shadow".
 - Walkable Material. Same as above but this one is used instead of the Shadow Material for the directions in which Wfc Voxel connections are walkable. There's the default one in the demo content with the name "walkable".
 - Starting Point. Coordinates from which the generation starts. In case "Grows" is disabled the default values are (0,0,0). Also if you want to use "Walkable Paths" and "Fixed Components" it's better to use this default value.
   In case of "Grows" selected the generation happens equally in all X and Y directions from this point.
 - Symmetric About The X Axis. Specifies if the right edge of the level should follow connection rules with the left edge by X axis.
 - Symmetric About The X Axis. Same as above but for the Y axis.
 - Generate In Play Mode. Deprecated option since the real-time generation is possible directly in the Editor. Doesn't do anything anymore.
 - Reject Non Walkable. You can set it when you have at least one walkable path set. Whan set don't leave any voxels in the final level which have walkbale connectors but are not connected to any walkable path.
 - Blueprint Editor Row Length. The number of Wfc Voxels which are put in a row in Blueprint Editor viewport. After this number the second row and further are started.
 - Show Placeholders. Visualize placeholders in Editor Viewport. More about placeholders under Placeholder section.
 - Show collapsed during generation. Spawns and removes actors/instanced static meshes during the generation process. It's a nice visualization of the generation process. Replaces deprecated "Generate In Play Mode".
 - Auto Map Components To Actors. Allows you to automatically map any Blueprints to the Wfc Voxel-s under you Wfc Actor. Your component name should be equal or start with the name of the required Blueprint.
 - Auto Map Exact Match. Limits the option above only to the exect matches.
 - Pairs. Manually specified pairs of Wfc Component and Blueprint names to map.
 
Advanced Controls (Generator Advanced section):
-------------
 - Next. Collapses the next N elements according to the default algorithm rules. N is set by "Step size".
 - Undo. Undoes the last N steps. N is set by "Undo size"
 - Retry If Step Fails. If set the Next button continues until you reach the generation + N from the current one even if there are failing attempts in the middle.


Wfc Voxel:
-------------
 - Name. The name used for the most of the Wfc Actor settings - for mapping to Blueprints, fixed components, etc.
 - Static Mesh. The mesh which will be used for this voxel if it's not mapped to Blueprint.
 - X/Y/Z Plus/Minus Direction. deprecated fields. They were previously used to set up the connectors but are now replaced by X/Y/Z Plus/Minus String which allows to enter not only numbers.
 - X/Y/Z Plus/Minus String. The identifier that specifies the connection in one of the 6 directions (could also be called "connector"). Sides of two Wfc Voxels can only be connected when the corresponding identifiers match.
 - X/Y Plus/Minus Direction Symmetry. Has 3 values:
     1) Connects to Itself means that every connector with the same value can be connected to this one.
     2) Straight - only Flipped connectors with the same number may connect here.
     3) Flipped - only Straight connectors with the same number may connect here.
 - Z Plus/Minus Direction Symmetry. May have 5 values:
     1) Any Rotate - rotation does not matter. Anything with the matching number may connect here regardless the rotation.
     2-5) Not Rotate and Rotate One/Two/Three Times - the specific rotation required to match this connector vertically. Example: when you set "Rotate One Time" in Z Plus direction this connector will match (among others) another Wfc Voxel with "Not Rotate" in Z Minus
     direction and rotated one time.
 - X/Y/Z Plus/Minus Walkable. Specifies if this side has a walkable connector. In the Blueprint Editor viewport you will see such connections with "walkable" material (green in the demo content)
 - Size. The default value is (1,1,1) which is the basic Wfc Voxel. If you set it bigger it becomes a "Big Tile" which has separate connectors for each child. But in general all children are st up the same way as regular Wfc Voxel-s.
 - Weight. The coefficient which indicated how often you want the algorithm to pick this specific Voxel. The higher the weight is the more often it will be picked (if the rules allow it).
 - X/Y/Z Plus/Minus Only Excluded/Allowed. You can specify manually the list with either names of the voxels that should not be connected here or the list of the only voxels that could be matched. These are basically black/white lists.

Placeholder (could also be applied to all the selected placeholders):
-------------
 - Random/<The Wfc Voxel name>. If the specific name is selected it will be used for "Collapse" or "Add to Fixed Components" (see below).
 - Collapse. Tell the lagorithm to collapse (and propagate afterward) at the selected position.
 - Add to Fixed Components. Add this component to the list of the fixed components of the parent Wfc Actor.
 - Remove. Removes this coordinates from the elements considered collapsed by the algorithm.
