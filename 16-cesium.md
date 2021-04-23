# Cesium

## 1 项目

- 下载项目

```shell
git clone https://github.com/ShareQiu1994/cesium-vue.git
```

## 2 操作

- 新建`viewer`并初始化

  ```js
  import { Viewer } from 'cesium'
  var viewer = new Viewer('cesiumContainer', {
        geocoder: false, // 聚焦
        homeButton: false, // home
        sceneModePicker: false, // 切换视角
        baseLayerPicker: false, // 切换图层
        navigationHelpButton: false, // help
        animation: false, // 速度控制球 左下角
        // creditContainer: 'credit',
        timeline: false, // 时间轴
        fullscreenButton: false, // 全屏
        vrButton: false // vr
      })
  ```

- 隐藏ui

  ```js
  // 帧数
  viewer.scene.debugShowFramesPerSecond = true
  // cesium版本等信息
  viewer._cesiumWidget._creditContainer.style.display = 'none'
  ```

- 绘制形状

  ```js
  import { Cartesian3, Viewer, Color } from 'cesium'
  
  var redBox = this.viewer.entities.add({
          name: 'Red box with black outline',
          position: Cartesian3.fromDegrees(-107.0, 40.0, 300000.0),
          box: {
              // 尺寸
            dimensions: new Cartesian3(400000.0, 300000.0, 500000.0),
            material: Color.RED.withAlpha(0.5),
            outline: true,
            outlineColor: Color.BLACK
          }
        })
  
        this.viewer.zoomTo(this.viewer.entities)
  ```

- 更换材料

  ```js
  //方法一，构造时赋材质
  var entity = viewer.entities.add({
    position: Cesium.Cartesian3.fromDegrees(-103.0, 40.0),
    ellipse : {
      semiMinorAxis : 250000.0,
      semiMajorAxis : 400000.0,
      material : Cesium.Color.BLUE.withAlpha(0.5)//可设置不同的MaterialProperty
    }
  });
  
  //方法二，构造后赋材质
  var ellipse = entity.ellipse;
  ellipse.material = Cesium.Color.RED;
  ```

  



