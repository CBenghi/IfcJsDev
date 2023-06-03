# Ifc js model load

## Geometry

### Demo web viewer implementation

 happens in `loadAllGeometry` function, which does some initialization, then uses the `state` to invoke `StreamAllMeshes`.
 Each mesh, is then loaded by streamMesh in the `main.js`.

 `StreamAllMeshes` is defined in both in js and wasm, it expects a callback function that receives the in the viewer. The call defines default type exclusion (e.g. spaces, voids and such). The call to `StreamAllMeshesWithTypes` returns the types requested, instead.

### Code

I think both js and c++ have the `LoadAllGeometry` (caps may vary).

The direct version of the calls return: `std::vector<webifc::geometry::IfcFlatMesh>`.

Both streaming and non-streaming use `webifc::geometry::IfcFlatMesh mesh = geomLoader->GetFlatMesh(id);`. I think there's a need to clean up the memory of the loader when this is completed.

### Classes

#### IfcComposedMesh

Holds a recursive geometry type that has the entity labels of one geometry and a list of children.
The main geometry has a transformation.
This holds, no vertex data, but it can be calculated with `IfcGeometry = processor.GetGeometry(mesh.expressID);`.
It also has a color (directly or through the children).

This is what the rather complicated function `IfcGeometryProcessor::GetMeshByLine` returns.

#### IfcFlatMesh

Just a collection of `IfcPlacedGeometry` plus an entity label. No vertex data.

#### IfcPlacedGeometry

Holds information about the entity labels of geometries and their placement, but no vertex data.

#### IfcGeometry

seems to hold actual vertex data, initialized by `geomLoader->GetGeometry(geom.geometryExpressID);`, vertex data is populated by `GetVertexData();`

`IfcGeometry` Inherits from  `fuzzybools::Geometry`, which is the tiny boolean library of IfcJs.

Writing to obj format is possible, using `ToObj()` in `io_helpers.cpp`.

Points and vertices can be navigated using `geom.GetPoint(i)`

#### NormalizeIFC

Is a matrix `glm::dmat4` that places inverted Y on Z and positive Z on y, because IFC is all rotated weirdly.

### TODO

- Objective: Regression suite
  - study the IfcFlatMesh class to see what are the possibilities to study the outcomes of the mesh generation.
  - hook the errorhandler code to catch problems when parsing the regression suite
