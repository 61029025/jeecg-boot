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

    // A demo of interactive 3D Tiles styling
    // Styling language Documentation: https://github.com/CesiumGS/3d-tiles/tree/master/specification/Styling
    // Building data courtesy of NYC OpenData portal: http://www1.nyc.gov/site/doitt/initiatives/3d-building.page
    var viewer = new Cesium.Viewer("cesiumContainer", {
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

    //去除版权信息
    viewer._cesiumWidget._creditContainer.style.display = "none";
    viewer.scene.globe.depthTestAgainstTerrain = true;

    // Add Cesium OSM Buildings, a global 3D buildings layer.
    viewer.scene.primitives.add(Cesium.createOsmBuildings({
      style : new Cesium.Cesium3DTileStyle({
        color: {
          conditions: [
            ["${feature['building']} === 'hospital'", "color('#0000FF')"],
            ["${feature['building']} === 'school'", "color('#00FF00')"],
            ["${name} === 'One World Trade Center'","color('#FF0000')"],
            [true, "color('#ffffff')"]
            // ['${height} >= 100', 'color("purple", 0.5)'],
            // ['${height} >= 50', 'color("red")'],
            // ['true', 'color("blue")']
          ],
        },
      })
    }));

    // Set the initial camera view to look at Manhattan
    var initialPosition = Cesium.Cartesian3.fromDegrees(
        -74.01881302800248,
        40.69114333714821,
        753
    );
    var initialOrientation = new Cesium.HeadingPitchRoll.fromDegrees(
        21.27879878293835,
        -21.34390550872461,
        0.0716951918898415
    );
    viewer.scene.camera.setView({
      destination: initialPosition,
      orientation: initialOrientation,
      endTransform: Cesium.Matrix4.IDENTITY,
    });

    // Load the NYC buildings tileset.
    // var tileset = new Cesium.Cesium3DTileset({
    //   url: Cesium.IonResource.fromAssetId(580348),
    // });
    // viewer.scene.primitives.add(tileset);

    // Color buildings based on their height.
    // function colorByHeight() {
    //   tileset.style = new Cesium.Cesium3DTileStyle({
    //     color: {
    //       conditions: [
    //         ["${Height} >= 300", "rgb(255,0,0)"],
    //         ["${Height} >= 200", "rgb(102, 71, 151)"],
    //         ["${Height} >= 100", "rgb(170, 162, 204)"],
    //         ["${Height} >= 50", "rgb(224, 226, 238)"],
    //         ["${Height} >= 25", "rgb(252, 230, 200)"],
    //         ["${Height} >= 10", "rgb(248, 176, 87)"],
    //         ["${Height} >= 5", "rgb(198, 106, 11)"],
    //         ["true", "rgb(127, 59, 8)"],
    //       ],
    //     },
    //   });
    // }
    //
    // // Color buildings by their latitude coordinate.
    // function colorByLatitude() {
    //   tileset.style = new Cesium.Cesium3DTileStyle({
    //     defines: {
    //       latitudeRadians: "radians(${Latitude})",
    //     },
    //     color: {
    //       conditions: [
    //         ["${latitudeRadians} >= 0.7125", "color('purple')"],
    //         ["${latitudeRadians} >= 0.712", "color('red')"],
    //         ["${latitudeRadians} >= 0.7115", "color('orange')"],
    //         ["${latitudeRadians} >= 0.711", "color('yellow')"],
    //         ["${latitudeRadians} >= 0.7105", "color('lime')"],
    //         ["${latitudeRadians} >= 0.710", "color('cyan')"],
    //         ["true", "color('blue')"],
    //       ],
    //     },
    //   });
    // }
    //
    // // Color buildings by distance from a landmark.
    // function colorByDistance() {
    //   tileset.style = new Cesium.Cesium3DTileStyle({
    //     defines: {
    //       distance:
    //           "distance(vec2(radians(${Longitude}), radians(${Latitude})), vec2(-1.291777521, 0.7105706624))",
    //     },
    //     color: {
    //       conditions: [
    //         ["${distance} > 0.0012", "color('gray')"],
    //         [
    //           "${distance} > 0.0008",
    //           "mix(color('yellow'), color('red'), (${distance} - 0.008) / 0.0004)",
    //         ],
    //         [
    //           "${distance} > 0.0004",
    //           "mix(color('green'), color('yellow'), (${distance} - 0.0004) / 0.0004)",
    //         ],
    //         ["${distance} < 0.00001", "color('white')"],
    //         [
    //           "true",
    //           "mix(color('blue'), color('green'), ${distance} / 0.0004)",
    //         ],
    //       ],
    //     },
    //   });
    // }
    //
    // // Color buildings with a '3' in their BIN property.
    // function colorByStringRegex() {
    //   tileset.style = new Cesium.Cesium3DTileStyle({
    //     color:
    //         "(regExp('3').test(String(${BIN}))) ? color('cyan', 0.9) : color('purple', 0.1)",
    //   });
    // }
    //
    // // Show only buildings greater than 200 meters in height.
    // function hideByHeight() {
    //   tileset.style = new Cesium.Cesium3DTileStyle({
    //     show: "${Height} > 200",
    //   });
    // }
    //
    // colorByHeight();

  }
}
</script>

<template>
  <div id="cesiumContainer"></div>
<!--  <iframe src="https://demos.cesium.com/NewYork/?view=-74.01881302800248%2C40.69114333714821%2C753.2406554180401%2C21.27879878293835%2C-21.343905508724625%2C0.0716951918898415"-->
<!--  style="height: 600px;width: 100%">-->
<!--  </iframe>-->
</template>

<style>

</style>
