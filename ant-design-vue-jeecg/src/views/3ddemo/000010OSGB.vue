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
    // Power Plant design model provided by Bentley Systems
    var viewer = new Cesium.Viewer("cesiumContainer",{
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
    //去除版权信息
    viewer._cesiumWidget._creditContainer.style.display = "none";
    var scene = viewer.scene;

    var tileset = scene.primitives.add(
      new Cesium.Cesium3DTileset({
        url: 'http://3dcloud.365power.cn/files/waters/model/tiles_zijin/tileset.json'
      })
    );

    tileset.readyPromise.then(function (tileset) {
      viewer.zoomTo(tileset, new Cesium.HeadingPitchRange(0.0, -0.5, 500));
    });
  }
}

</script>

<template>
  <div id="cesiumContainer"></div>
</template>

<style>

</style>
