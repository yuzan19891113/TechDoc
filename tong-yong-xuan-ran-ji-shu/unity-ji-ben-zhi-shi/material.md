# Material

 如果我们需要在脚本中访问**共享材质属性**，这里非常要注意一点，我们如果通过**Renderer.Material**来更改的话，会在内存中重新创建一个Material的副本，会带来内存的消耗。但是我们可以通过修改**Renderer.sharedMaterial**来修改，这样依然可以保持两个物体共享一个材质

