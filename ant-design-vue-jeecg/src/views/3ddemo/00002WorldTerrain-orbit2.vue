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
    var viewer = new Cesium.Viewer("cesiumContainer", {
      terrainProvider: Cesium.createWorldTerrain({
        requestVertexNormals: true,
        requestWaterMask: true
      }),
      animation: false, //是否显示动画控件，左下角仪表
      baseLayerPicker: false, //是否显示图层选择控件,是否显示geocoder小器件，右上角查询按钮
      geocoder: false, //是否显示地名查找控件
      timeline: false, //是否显示时间线控件
      sceneModePicker: false, //是否显示投影方式控件,//是否显示3D/2D选择器
      navigationHelpButton: false, //是否显示帮助信息控件
      infoBox: false, //是否显示点击要素之后显示的信息
      fullscreenButton: false, //是否显示全屏按钮
      selectionIndicator: false, //是否显示选中指示框
    });

    viewer._cesiumWidget._creditContainer.style.display = "none";

    // Lock camera to a point
    var center = Cesium.Cartesian3.fromRadians(2.4213211833389243, 0.6171926869414084, 3626.0426275055174);
    var transform = Cesium.Transforms.eastNorthUpToFixedFrame(center);
    viewer.scene.camera.lookAtTransform(transform, new Cesium.HeadingPitchRange(0, -Math.PI/8, 8000));

    // Orbit this point
    viewer.clock.onTick.addEventListener(function(clock) {
      viewer.scene.camera.rotateRight(0.005);
    });
    //https://sandcastle.cesium.com/?src=Multiple%20Synced%20Views.html
  }
}
</script>

<template>
  <div id="cesiumContainer"></div>
</template>

<style>

</style>
