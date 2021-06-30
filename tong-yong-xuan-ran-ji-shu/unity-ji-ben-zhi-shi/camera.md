# Camera

* 相机Position和Target设置，Position直接修改对应相机gameobj的transform,target直接修改position的lookat
* OrthProjection（正交投影\):Orthgraphicsize修改正交投影矩形的垂直半长，aspect决定水平半长与垂直半长的比例
* Camera.render有效执行只有一次，如果需要手动调用camera.render需要把camera.enabled = false

