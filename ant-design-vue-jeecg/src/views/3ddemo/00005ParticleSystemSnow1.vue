<script>
// This starter template is using Vue 3 <script setup> SFCs
// Check out https://v3.vuejs.org/api/sfc-script-setup.html#sfc-script-setup
import * as Cesium from "cesium"
import "@plugins/cesium/Widgets/widgets.css";
import snowflake_particle from "@assets/snowflake_particle.png"
import {geo_server_url} from "@/geoserver.config";


export default {
  data() {
    return {}
  },
  mounted() {
    var viewer = new Cesium.Viewer("cesiumContainer", {
      shouldAnimate: true,
      terrainProvider: Cesium.createWorldTerrain(),
      animation: false, //是否显示动画控件，左下角仪表
      baseLayerPicker: false, //是否显示图层选择控件,是否显示geocoder小器件，右上角查询按钮
      geocoder: false, //是否显示地名查找控件
      timeline: false, //是否显示时间线控件
      sceneModePicker: true, //是否显示投影方式控件,//是否显示3D/2D选择器
      navigationHelpButton: false, //是否显示帮助信息控件
      infoBox: false, //是否显示点击要素之后显示的信息
      fullscreenButton: true, //是否显示全屏按钮
      selectionIndicator: false, //是否显示选中指示框
    });
    //去除版权信息
    viewer._cesiumWidget._creditContainer.style.display = "none";
    var scene = viewer.scene;
    scene.globe.depthTestAgainstTerrain = true;
    var resetCameraFunction = function () {
      scene.camera.setView({
        destination: Cesium.Cartesian3.fromDegrees(111.208091, 37.051119,10000),
        orientation: {
          heading: 0,
          pitch: -Math.PI/6
        },
      });
    };
    resetCameraFunction();

    var token = '60f47949d8f34b4be16007a916eac3cb';
    // // 服务域名
    var tdtUrl = 'http://t{s}.tianditu.com/';
    var subdomains = ['0', '1', '2', '3', '4', '5', '6', '7'];

    //地名注记
    viewer.imageryLayers.addImageryProvider(
        new Cesium.WebMapTileServiceImageryProvider({
          url:
              tdtUrl + "cta_w/wmts?service=wmts&request=GetTile&version=1.0.0&LAYER=cta&tileMatrixSet=w&TileMatrix={TileMatrix}&TileRow={TileRow}&TileCol={TileCol}&style=default.jpg&tk=" + token,
          subdomains: subdomains,
          layer: "tdtAnnoLayer",
          style: "default",
          format: "tiles",
          tileMatrixSetID: "GoogleMapsCompatible",
          show: true
        })
    );

    //加载业务图层
    viewer.imageryLayers.addImageryProvider(
        new Cesium.WebMapServiceImageryProvider({
          url: geo_server_url + "PostGis/wms",
          layers: 'PostGis:g_d_dxd',
          layer: "bs",
          parameters: {
            service: 'WMS',
            format: 'image/png',
            transparent: true,
            srs: 'EPSG:4326',
          }
        })
    );

// snow
    var snowParticleSize = 12.0;
    var snowRadius = 100000.0;
    var minimumSnowImageSize = new Cesium.Cartesian2(
        snowParticleSize,
        snowParticleSize
    );
    var maximumSnowImageSize = new Cesium.Cartesian2(
        snowParticleSize * 2.0,
        snowParticleSize * 2.0
    );
    var snowSystem;

    var snowGravityScratch = new Cesium.Cartesian3();
    var snowUpdate = function (particle, dt) {
      snowGravityScratch = Cesium.Cartesian3.normalize(
          particle.position,
          snowGravityScratch
      );
      Cesium.Cartesian3.multiplyByScalar(
          snowGravityScratch,
          Cesium.Math.randomBetween(-30.0, -200.0),
          snowGravityScratch
      );
      particle.velocity = Cesium.Cartesian3.add(
          particle.velocity,
          snowGravityScratch,
          particle.velocity
      );

      var distance = Cesium.Cartesian3.distance(
          scene.camera.position,
          particle.position
      );
      if (distance > snowRadius) {
        particle.endColor.alpha = 0.0;
      } else {
        particle.endColor.alpha =
            snowSystem.endColor.alpha / (distance / snowRadius + 0.1);
      }
    };

    snowSystem = new Cesium.ParticleSystem({
      modelMatrix: new Cesium.Matrix4.fromTranslation(
          scene.camera.position
      ),
      minimumSpeed: -1.0,
      maximumSpeed: 0.0,
      lifetime: 15.0,
      emitter: new Cesium.SphereEmitter(snowRadius),
      startScale: 0.5,
      endScale: 1.0,
      image: snowflake_particle,
      emissionRate: 9000.0,
      startColor: Cesium.Color.WHITE.withAlpha(0.0),
      endColor: Cesium.Color.WHITE.withAlpha(1.0),
      minimumImageSize: minimumSnowImageSize,
      maximumImageSize: maximumSnowImageSize,
      updateCallback: snowUpdate,
    });
    scene.primitives.add(snowSystem);
  }
}
</script>

<template>
  <div id="cesiumContainer"></div>
</template>

<style>

</style>
