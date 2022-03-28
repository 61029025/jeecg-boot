<script>
// This starter template is using Vue 3 <script setup> SFCs
// Check out https://v3.vuejs.org/api/sfc-script-setup.html#sfc-script-setup
import * as Cesium from "cesium"
import "@plugins/cesium/Widgets/widgets.css";


export default {
  data() {
    return {}
  },
  mounted() {
    let old_token = Cesium.Ion.defaultAccessToken
    Cesium.Ion.defaultAccessToken = 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJqdGkiOiI3ODZkMDQzOS03ZGJjLTQzZWUtYjlmYy04ZmM5Y2UwNzNhMmYiLCJpZCI6MjU5LCJpYXQiOjE2MzgyMDYwMDB9.cK1hsaFBgz0l2dG9Ry5vBFHWp-HF2lwjLC0tcK8Z8tY'
    const viewer = new Cesium.Viewer("cesiumContainer",
    {
      terrainProvider: Cesium.createWorldTerrain(),
      animation: false, //是否显示动画控件，左下角仪表
        baseLayerPicker: false, //是否显示图层选择控件,是否显示geocoder小器件，右上角查询按钮
      geocoder: false, //是否显示地名查找控件
      timeline: false, //是否显示时间线控件
      sceneModePicker: true, //是否显示投影方式控件,//是否显示3D/2D选择器
      navigationHelpButton: false, //是否显示帮助信息控件
      infoBox: true, //是否显示点击要素之后显示的信息
      fullscreenButton: true, //是否显示全屏按钮
      selectionIndicator: true, //是否显示选中指示框
    });
    viewer._cesiumWidget._creditContainer.style.display = "none";
    //Point Cloud by Prof. Peter Allen, Columbia University Robotics Lab. Scanning by Alejandro Troccoli and Matei Ciocarlie.
    const tileset = new Cesium.Cesium3DTileset({
      url: 'http://3dcloud.365power.cn/files/epc/models/pointclouds/ground/tileset.json'
    });
    viewer.scene.primitives.add(tileset);
    tileset.readyPromise.then(function (tileset) {
      viewer.zoomTo(tileset, new Cesium.HeadingPitchRange(0.0, -0.5, 2000));
    });
    Cesium.Ion.defaultAccessToken = old_token
  }
}

</script>

<template>
  <div id="cesiumContainer"></div>
</template>

<style>
#toolbar {
  background: rgba(42, 42, 42, 0.8);
  padding: 4px;
  border-radius: 4px;
}
#toolbar input {
  vertical-align: middle;
  padding-top: 2px;
  padding-bottom: 2px;
}
#toolbar .header {
  font-weight: bold;
}
</style>
