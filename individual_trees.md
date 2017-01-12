# Support of natural=tree and natural=tree_row

[natural=tree](http://wiki.openstreetmap.org/wiki/Tag:natural%3Dtree) is used only for OSM nodes.<br>
[natural=tree_row](http://wiki.openstreetmap.org/wiki/Tag:natural%3Dtree_row) is used only for OSM ways.

If terrain is set (see [#49](https://github.com/vvoovv/blender-osm/issues/49)), then place them on the terrain. If terrain isn't set, then set z=0 for them.

The task of supporting _natural=tree_row_ can be decomposed to placing _natural=tree_ N times along an OSM way. The OSM way can be closed or have open ends. In the latter case a tree must be placed at both open ends. The distance between the row of trees should have a random variation. The trees should have some random deviation from the OSM way. The height and the width of the row of trees should have a random variation. The distance between the trees is more tricky to calculate in the presence of the terrain.

### 3D models for trees
I suggest to to place all tree models in a single .blend file. The name of the Blender object and Blender mesh for a tree should reflect the name of the genus or the species and how detailed the tree model is. A user can provide his own .blend file with tree models which must follow the convention for naming.

### Implemenation details

Create a _TreeManager_ class with _parseNode(self, element, elementId)_ and _parseWay(self, element, elementId)_. Those methods are responsible for checking if the related OSM node or way is valid.

Create a _TreeRenderer_ class (probaly derived from _renderer.Renderer_ class) resposible for loading or reusing the Blender mesh for a tree and placing it in the Blender scene. The class should have the method _renderNode(self, node, osm)_ to render a single tree and the method _renderLineString(self, rel, osm)_ to render a row tree. Them method _renderLineString(self, rel, osm)_ calls the method _renderNode(self, node, osm)_ to render each of row trees. Check if the methods _preRender(self, element, layerIndex=None)_ and _postRender(self, element, layerIndex=None)_ must be overriden in the class.

Alternatives to consider how to place a tree model:
* Direct placement (reuse the mesh for a tree, keep the trees as separate Blender objects, apply some scale to achive random variation OR may be join all tree Blender object into the single Blender object)
* Use Blender particles: create a proxy Blender mesh where each vertex corresponds to the position of a tree; then apply Blender particles to the proxy Blender object; in the Blender particles check how to achive random variation
* Use Blender DupliVerts: create a proxy Blender mesh like for in the case of Blender particles. Random variation doesn't seem to be possible

For the case of direct placement see _building/roof/RoofMesh.render(..)_ how to load a Blender mesh and reuse the existing mesh. Maybe that code should be moved to _util/blender_ to be used by other code parts

### Placement on the terrain
Use [bpy.types.Object.ray_cast(..)](https://www.blender.org/api/blender_python_api_current/bpy.types.Object.html#bpy.types.Object.ray_cast) or [mathutils.bvhtree.BVHTree.ray_cast(..)](https://www.blender.org/api/blender_python_api_current/mathutils.bvhtree.html#mathutils.bvhtree.BVHTree.ray_cast)? To be decided in terms of performance. Probably the latter one should be faster due to only one initialization instead of initialization overhead on each call for the former one.
