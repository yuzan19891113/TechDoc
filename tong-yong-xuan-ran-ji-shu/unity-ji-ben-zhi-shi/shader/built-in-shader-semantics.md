# Built-in shader semantics

### Build in shader input layout

* struct `appdata_base`: **vertex shader**  input with position, normal, one texture coordinate.
* struct `appdata_tan`: vertex shader input with position, normal, tangent, one texture coordinate.
* struct `appdata_full`: vertex shader input with position, normal, tangent, vertex color and two texture coordinates.
* struct `appdata_img`: vertex shader input with position and one texture coordinate.

### CG Macro

* CGProgram- ENDCG：包含的是\#pragma shader
* CGINCLUDE- ENDCG：包含的是 shader 本体与变量定义

