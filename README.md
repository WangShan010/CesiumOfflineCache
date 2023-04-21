# ~~CesiumOfflineCache~~









> 本仓库已废弃，请关注新仓库：https://github.com/WangShan010/CesiumNetworkPlug










这是一个 Cesium 插件，它使用 **indexDB** 离线缓存技术管理 影像图层、地形、3DTiles模型 等资源数据。

作者 QQ：2394837320

模块内置授权模块，试用期到 2023年04月1日，如有意愿可联系作者购买源码



## 一、运行原理

在Cesium 通过 Cesium.Resource 发送 **资源请求**（图层、地形、模型）前，判断本地是否有缓存存在，如果存在则优先使用本地缓存，本地缓存能极大加快场景的二次加载速度。



## 二、起步

### 2.1、引入插件

- script 方式引入

```html
<script src="./CesiumOfflineCache.min.js"></script>
```



- ES6 方式引入

```html
<script type="module">
    import './CesiumOfflineCache.min.js';
</script>
```



### 2.2、使用方式

- index.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="./Cesium/^1.98/Cesium.js"></script>
    <link rel="stylesheet" href="./Cesium/^1.98/Widgets/widgets.css">
</head>
<body>
<div id="MapContainer" style="width: 100%;height: 100%"></div>
<script type="module" src="./js/main.js"></script>
</body>
</html>
```



- main.js

```js
import './CesiumOfflineCache.min.js';

CesiumOfflineCache.ruleList.add('*');
let viewer = new Cesium.Viewer('MapContainer', {
    imageryProvider: new Cesium.UrlTemplateImageryProvider({
        url: 'https://c.tile.thunderforest.com/transport/{z}/{x}/{y}.png',
        tilingScheme: new Cesium.WebMercatorTilingScheme(),
        maximumLevel: 19
    }),
    timeline: false,
    animation: false
});

viewer.scene.camera.setView({
    destination: Cesium.Cartesian3.fromDegrees(-122.3472, 47.598, 370),
    orientation: {
        heading: Cesium.Math.toRadians(10),
        pitch: Cesium.Math.toRadians(-10)
    }
});

setTimeout(() => {
    let tileSet = Cesium.createOsmBuildings();
    viewer.scene.primitives.add(tileSet);
}, 3000);

```





## 三、API 文档

命名空间：`window.CesiumOfflineCache.*`



```js
const CesiumOfflineCache = {
    // 缓存规则
    ruleList: new Set(),
    // 判断该资源项是否符合缓存规则
    judgeUrl(url) {},
    async getItem(k) {},
    async setItem(k, v) {},
    async keys() {},
    async clear() {},
    // 计算缓存占用的存储空间大小
    async getUseSize() {}
};
```



- 全局缓存

```js
CesiumOfflineCache.ruleList.add('*');
```

- 对 OSM 电子地图缓存

```js
CesiumOfflineCache.ruleList.add('https://c.tile.thunderforest.com/');
```

- 对指定地址的 3DTile 缓存

```js
CesiumOfflineCache.ruleList.add('http://xxx.xx.xx.xx:3000//3DTiles/');
```
