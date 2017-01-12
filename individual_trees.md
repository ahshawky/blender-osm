# Support of natural=tree and natural=tree_row

[natural=tree](http://wiki.openstreetmap.org/wiki/Tag:natural%3Dtree) is used only for OSM nodes.<br>
[natural=tree_row](http://wiki.openstreetmap.org/wiki/Tag:natural%3Dtree_row) is used only for OSM ways.

If terrain is set (see [#49](https://github.com/vvoovv/blender-osm/issues/49)), then place them on the terrain. If terrain isn't set, then set z=0 for them.

The task of supporting _natural=tree_row_ can be decomposed to placing _natural=tree_ N times along an OSM way. The OSM way can be closed or have open ends. In the latter case a tree must be placed at both open ends. The distance between the row of trees should have a random variation. The trees should have some random deviation from the OSM way. The height and the width of the row of trees should have a random variation. The distance between the trees is more tricky to calculate in the presence of the terrain.

I suggest to to place all tree models in a single .blend file. The name of the Blender object and Blender mesh for a tree should reflect the name of the genus or the species and how detailed the tree model is. A user can provide his own .blend file with tree models which must follow the convention for naming.

### Placement on the terrain
Use [bpy.types.Object.ray_cast(..)](https://www.blender.org/api/blender_python_api_current/bpy.types.Object.html#bpy.types.Object.ray_cast) or [mathutils.bvhtree.BVHTree.ray_cast(..)](https://www.blender.org/api/blender_python_api_current/mathutils.bvhtree.html#mathutils.bvhtree.BVHTree.ray_cast)? To be decided in terms of performance. Probably the latter one should be faster due to only one initialization instead of initialization overhead on each call for the former one.
