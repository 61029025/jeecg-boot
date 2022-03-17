<script>

import {
  WebMapTileServiceImageryProvider,
  Cartesian3,
  Viewer,
  Math
} from "cesium"
import "@plugins/cesium/Widgets/widgets.css";
import Cesium from "cesium";


export default {
  data() {
    return {}
  },
  mounted() {
    var token = '60f47949d8f34b4be16007a916eac3cb';
    // // 服务域名
    var tdtUrl = 'https://t0.tianditu.gov.cn/';

    var viewer = new Viewer("cesiumContainer", {
      // imageryProvider: earth, //地图数据
      animation: false, //是否显示动画控件，左下角仪表
      baseLayerPicker: false, //是否显示图层选择控件,是否显示geocoder小器件，右上角查询按钮
      geocoder: false, //是否显示地名查找控件
      timeline: false, //是否显示时间线控件
      sceneModePicker: true, //是否显示投影方式控件,//是否显示3D/2D选择器
      navigationHelpButton: false, //是否显示帮助信息控件
      infoBox: false, //是否显示点击要素之后显示的信息
      fullscreenButton: true, //是否显示全屏按钮
      selectionIndicator: false, //是否显示选中指示框
      // imageryProviderViewModels: [img_arcgis_yxdt, img_arcgis_jcdt, img_tdt_sl, img_tdt_yx],//可供BaseLayerPicker选择的图像图层ProviderViewModel数组
      // selectedImageryProviderViewModel: img_arcgis_jcdt,//当前地形图层的显示模型，仅baseLayerPicker设为true有意义
      // scene3DOnly: true, //如果设置为true，则所有几何图形以3D模式绘制以节约GPU资源
      // homeButton: false, //是否显示Home按钮
      // showRenderLoopErrors: false //如果设为true，将在一个HTML面板中显示错误信息
    });
    //去除版权信息
    viewer._cesiumWidget._creditContainer.style.display = "none";
    //矢量底图
    viewer.imageryLayers.addImageryProvider(
      new WebMapTileServiceImageryProvider({
        url:
          "http://t0.tianditu.com/vec_w/wmts?service=wmts&request=GetTile&version=1.0.0&LAYER=vec&tileMatrixSet=w&TileMatrix={TileMatrix}&TileRow={TileRow}&TileCol={TileCol}&style=default.jpg&tk=" + token,
        layer: "tdtImgBasicLayer",
        style: "default",
        format: "image/jpeg",
        tileMatrixSetID: "GoogleMapsCompatible",
        show: false
        // maximumLevel: 18
      })
    );
    //中文地名注记
    viewer.imageryLayers.addImageryProvider(
      new WebMapTileServiceImageryProvider({
        url:
          "http://t0.tianditu.com/cia_w/wmts?service=wmts&request=GetTile&version=1.0.0&LAYER=cia&tileMatrixSet=w&TileMatrix={TileMatrix}&TileRow={TileRow}&TileCol={TileCol}&style=default.jpg&tk=" + token,
        layer: "tdtAnnoLayer",
        style: "default",
        format: "tiles",
        tileMatrixSetID: "GoogleMapsCompatible",
        show: true
      })
    );


    // 将三维球定位到中国
    viewer.camera.flyTo({
      destination: Cartesian3.fromDegrees(103.84, 31.15, 17850000),
      orientation: {
        heading: Math.toRadians(348.4202942851978),
        pitch: Math.toRadians(-89.74026687972041),
        roll: Math.toRadians(0)
      },
      complete: function callback() {
        // 定位完成之后的回调函数
        // Fly the camera to San Francisco at the given longitude, latitude, and height.
        viewer.camera.flyTo({
          destination: Cartesian3.fromDegrees(117.19632254805451, 39.110371612761384, 2000),
          orientation: {
            heading: Math.toRadians(0),
            pitch: Math.toRadians(-30.0),
          }
        });
      }
    });
  }
}
</script>

<template>
  <div id="cesiumContainer"></div>
</template>

<style>

</style>
