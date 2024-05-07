The Simplest Graph Renderer there is

Dependencies:

- GLEW
- OpenGL
- GLFW

To build super lazy on Linux:

```bash
./build.sh
```

To build:

```bash
mkdir build
cd build
cmake ..
```

To run (in build):

```bash
./simple -gf <path-to-graph-file>
```

Expected Graph Format:

```
<Vertex Count>
<Edge Count>
<Vertex 1>
<Vertex 2>
...
<Vertex N>
<Edge 1>
<Edge 2>
...
<Edge M>
```

Verticies and Edges are defined as follows

```
<Vertex> := <Longitude> <Latitude>
<Edge>   := <Start Vertex ID> <End Vertex ID> <Width> <Color>
```

Except `<Longitude>` and `<Latitude>` which may contain Double values, all other data fields only allow Integer values.

Color Values for `<Color>` are
- 0 - Black
- 1 - Light Blue
- 2 - Red
- 3 - Yellow
- 4 - Light Yellow
- 5 - White
