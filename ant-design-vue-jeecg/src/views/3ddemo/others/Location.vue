<!--<script>-->
<!--// This starter template is using Vue 3 <script setup> SFCs-->
<!--// Check out https://v3.vuejs.org/api/sfc-script-setup.html#sfc-script-setup-->
<!--import * as Cesium from "cesium"-->
<!--import "@plugins/cesium/Widgets/widgets.css";-->
<!--import {geo_server_url} from "@/geoserver.config";-->


<!--export default {-->
<!--  data() {-->
<!--    return {-->
<!--      lon:111.208091,-->
<!--      lat:37.091119,-->
<!--      height:20000,-->
<!--      viewer:null-->
<!--    }-->
<!--  },-->
<!--  mounted() {-->

<!--    // Initialize the Cesium Viewer in the HTML element with the `cesiumContainer` ID.-->
<!--    this.viewer = new Cesium.Viewer('cesiumContainer', {-->
<!--      terrainProvider: Cesium.createWorldTerrain(),-->
<!--      animation: false, //是否显示动画控件，左下角仪表-->
<!--      baseLayerPicker: false, //是否显示图层选择控件,是否显示geocoder小器件，右上角查询按钮-->
<!--      geocoder: false, //是否显示地名查找控件-->
<!--      timeline: false, //是否显示时间线控件-->
<!--      sceneModePicker: true, //是否显示投影方式控件,//是否显示3D/2D选择器-->
<!--      navigationHelpButton: false, //是否显示帮助信息控件-->
<!--      infoBox: false, //是否显示点击要素之后显示的信息-->
<!--      fullscreenButton: true, //是否显示全屏按钮-->
<!--      selectionIndicator: false, //是否显示选中指示框-->
<!--    });-->

<!--    // Add Cesium OSM Buildings, a global 3D buildings layer.-->
<!--    this.viewer.scene.primitives.add(Cesium.createOsmBuildings());-->
<!--    this.viewer._cesiumWidget._creditContainer.style.display = "none";-->

<!--    //加载业务图层-->
<!--    this.viewer.imageryLayers.addImageryProvider(-->
<!--        new Cesium.WebMapServiceImageryProvider({-->
<!--          url: geo_server_url + "PostGis/wms",-->
<!--          layers: 'PostGis:g_d_dxd',-->
<!--          layer: "bs",-->
<!--          parameters: {-->
<!--            service: 'WMS',-->
<!--            format: 'image/png',-->
<!--            transparent: true,-->
<!--            srs: 'EPSG:4326',-->
<!--          }-->
<!--        })-->
<!--    );-->
<!--    // Fly the camera to San Francisco at the given longitude, latitude, and height.-->
<!--    this.viewer.camera.flyTo({-->
<!--      destination: Cesium.Cartesian3.fromDegrees(-122.4175, 37.655, 400),-->
<!--      orientation: {-->
<!--        heading: Cesium.Math.toRadians(0.0),-->
<!--        pitch: Cesium.Math.toRadians(-15.0),-->
<!--      }-->
<!--    });-->
<!--  },-->
<!--  methods:{-->
<!--    query() {-->
<!--      const vm = this-->
<!--      debugger-->
<!--      this.viewer.camera.flyTo({-->
<!--        destination: Cesium.Cartesian3.fromDegrees(vm.lon, vm.lat, vm.height),-->
<!--        orientation: {-->
<!--          heading: Cesium.Math.toRadians(0.0),-->
<!--          pitch: Cesium.Math.toRadians(-40.0),-->
<!--        }-->
<!--      });-->
<!--    }-->
<!--  }-->
<!--}-->
<!--</script>-->

<!--<template>-->
<!--  <el-row>-->
<!--    <el-col :span="6">-->
<!--      <el-input v-model="lon" placeholder="请输入经度">-->
<!--        <template #prepend>经度</template>-->
<!--      </el-input>-->
<!--    </el-col>-->
<!--    <el-col :span="6">-->
<!--      <el-input v-model="lat" placeholder="请输入纬度">-->
<!--        <template #prepend>纬度</template>-->
<!--      </el-input>-->
<!--    </el-col>-->
<!--    <el-col :span="6">-->
<!--      <el-input v-model="height" placeholder="请输入高度">-->
<!--        <template #prepend>高度</template>-->
<!--      </el-input>-->
<!--    </el-col>-->
<!--    <el-col :span="6">-->
<!--      <el-button @click="query">定位</el-button>-->
<!--    </el-col>-->
<!--  </el-row>-->
<!--  <div id="cesiumContainer"></div>-->
<!--</template>-->

<!--<style>-->

<!--</style>-->
