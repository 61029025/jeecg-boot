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
    Cesium.Math.setRandomNumberSeed(1234);

    var viewer = new Cesium.Viewer("cesiumContainer", {
      infoBox: false,
      animation: false, //是否显示动画控件，左下角仪表
      baseLayerPicker: false, //是否显示图层选择控件,是否显示geocoder小器件，右上角查询按钮
      geocoder: false, //是否显示地名查找控件
      timeline: false, //是否显示时间线控件
      sceneModePicker: true, //是否显示投影方式控件,//是否显示3D/2D选择器
      navigationHelpButton: false, //是否显示帮助信息控件
      fullscreenButton: true, //是否显示全屏按钮
      selectionIndicator: false, //是否显示选中指示框
    });
    //去除版权信息
    viewer._cesiumWidget._creditContainer.style.display = "none";

    var entities = viewer.entities;

    var i;
    var height;
    var positions;
    var stripeMaterial = new Cesium.StripeMaterialProperty({
      evenColor: Cesium.Color.WHITE.withAlpha(0.5),
      oddColor: Cesium.Color.BLUE.withAlpha(0.5),
      repeat: 5.0,
    });

    entities.add({
      rectangle: {
        coordinates: Cesium.Rectangle.fromDegrees(-92.0, 20.0, -86.0, 27.0),
        outline: true,
        outlineColor: Cesium.Color.WHITE,
        outlineWidth: 4,
        stRotation: Cesium.Math.toRadians(45),
        material: stripeMaterial,
      },
    });

    entities.add({
      polygon: {
        hierarchy: new Cesium.PolygonHierarchy(
            Cesium.Cartesian3.fromDegreesArray([
              -107.0,
              27.0,
              -107.0,
              22.0,
              -102.0,
              23.0,
              -97.0,
              21.0,
              -97.0,
              25.0,
            ])
        ),
        outline: true,
        outlineColor: Cesium.Color.WHITE,
        outlineWidth: 4,
        material: stripeMaterial,
      },
    });

    entities.add({
      position: Cesium.Cartesian3.fromDegrees(-80.0, 25.0),
      ellipse: {
        semiMinorAxis: 300000.0,
        semiMajorAxis: 500000.0,
        rotation: Cesium.Math.toRadians(-40.0),
        outline: true,
        outlineColor: Cesium.Color.WHITE,
        outlineWidth: 4,
        stRotation: Cesium.Math.toRadians(22),
        material: stripeMaterial,
      },
    });

    entities.add({
      position: Cesium.Cartesian3.fromDegrees(-72.0, 25.0),
      ellipse: {
        semiMinorAxis: 250000.0,
        semiMajorAxis: 250000.0,
        rotation: Cesium.Math.toRadians(-40.0),
        outline: true,
        outlineColor: Cesium.Color.WHITE,
        outlineWidth: 4,
        stRotation: Cesium.Math.toRadians(90),
        material: stripeMaterial,
      },
    });

    entities.add({
      rectangle: {
        coordinates: Cesium.Rectangle.fromDegrees(
            -118.0,
            38.0,
            -116.0,
            40.0
        ),
        extrudedHeight: 500000.0,
        outline: true,
        outlineColor: Cesium.Color.WHITE,
        outlineWidth: 4,
        stRotation: Cesium.Math.toRadians(45),
        material: Cesium.Color.fromRandom({ alpha: 1.0 }),
      },
    });

    entities.add({
      position: Cesium.Cartesian3.fromDegrees(-117.0, 35.0),
      ellipse: {
        semiMinorAxis: 100000.0,
        semiMajorAxis: 200000.0,
        height: 100000.0,
        extrudedHeight: 200000.0,
        rotation: Cesium.Math.toRadians(90.0),
        outline: true,
        outlineColor: Cesium.Color.WHITE,
        outlineWidth: 4,
        material: Cesium.Color.fromRandom({ alpha: 1.0 }),
      },
    });

    entities.add({
      polygon: {
        hierarchy: new Cesium.PolygonHierarchy(
            Cesium.Cartesian3.fromDegreesArray([
              -118.0,
              30.0,
              -115.0,
              30.0,
              -117.1,
              31.1,
              -118.0,
              33.0,
            ])
        ),
        height: 300000.0,
        extrudedHeight: 700000.0,
        outline: true,
        outlineColor: Cesium.Color.WHITE,
        outlineWidth: 4,
        material: Cesium.Color.fromRandom({ alpha: 1.0 }),
      },
    });

    entities.add({
      position: Cesium.Cartesian3.fromDegrees(-70.0, 45.0, 100000.0),
      cylinder: {
        hierarchy: new Cesium.PolygonHierarchy(
            Cesium.Cartesian3.fromDegreesArray([
              -118.0,
              30.0,
              -115.0,
              30.0,
              -117.1,
              31.1,
              -118.0,
              33.0,
            ])
        ),
        length: 200000.0,
        topRadius: 150000.0,
        bottomRadius: 150000.0,
        outline: true,
        outlineColor: Cesium.Color.WHITE,
        outlineWidth: 4,
        material: Cesium.Color.fromRandom({ alpha: 1.0 }),
      },
    });

    for (i = 0; i < 5; ++i) {
      height = 100000.0 + 200000.0 * i;
      entities.add({
        position: Cesium.Cartesian3.fromDegrees(-106.0, 45.0, height),
        box: {
          dimensions: new Cesium.Cartesian3(90000.0, 90000.0, 90000.0),
          outline: true,
          outlineColor: Cesium.Color.WHITE,
          outlineWidth: 2,
          material: Cesium.Color.fromRandom({ alpha: 0.5 }),
        },
      });

      entities.add({
        position: Cesium.Cartesian3.fromDegrees(-102.0, 45.0, height),
        ellipsoid: {
          radii: new Cesium.Cartesian3(45000.0, 45000.0, 90000.0),
          outline: true,
          outlineColor: Cesium.Color.WHITE,
          outlineWidth: 2,
          material: Cesium.Color.fromRandom({ alpha: 0.5 }),
        },
      });

      entities.add({
        position: Cesium.Cartesian3.fromDegrees(-98.0, 45.0, height),
        ellipsoid: {
          radii: new Cesium.Cartesian3(67500.0, 67500.0, 67500.0),
          outline: true,
          outlineColor: Cesium.Color.WHITE,
          outlineWidth: 2,
          material: Cesium.Color.fromRandom({ alpha: 0.5 }),
        },
      });
    }

    entities.add({
      wall: {
        positions: Cesium.Cartesian3.fromDegreesArray([
          -95.0,
          50.0,
          -85.0,
          50.0,
          -75.0,
          50.0,
        ]),
        maximumHeights: [500000, 1000000, 500000],
        minimumHeights: [0, 500000, 0],
        outline: true,
        outlineColor: Cesium.Color.LIGHTGRAY,
        outlineWidth: 4,
        material: Cesium.Color.fromRandom({ alpha: 0.7 }),
      },
    });

    entities.add({
      rectangle: {
        coordinates: Cesium.Rectangle.fromDegrees(-92.0, 30.0, -85.0, 40.0),
        material: stripeMaterial,
      },
    });

    entities.add({
      polygon: {
        hierarchy: {
          positions: Cesium.Cartesian3.fromDegreesArray([
            -109.0,
            30.0,
            -95.0,
            30.0,
            -95.0,
            40.0,
            -109.0,
            40.0,
          ]),
          holes: [
            {
              positions: Cesium.Cartesian3.fromDegreesArray([
                -107.0,
                31.0,
                -107.0,
                39.0,
                -97.0,
                39.0,
                -97.0,
                31.0,
              ]),
              holes: [
                {
                  positions: Cesium.Cartesian3.fromDegreesArray([
                    -105.0,
                    33.0,
                    -99.0,
                    33.0,
                    -99.0,
                    37.0,
                    -105.0,
                    37.0,
                  ]),
                  holes: [
                    {
                      positions: Cesium.Cartesian3.fromDegreesArray([
                        -103.0,
                        34.0,
                        -101.0,
                        34.0,
                        -101.0,
                        36.0,
                        -103.0,
                        36.0,
                      ]),
                    },
                  ],
                },
              ],
            },
          ],
        },
        material: stripeMaterial,
      },
    });

    entities.add({
      position: Cesium.Cartesian3.fromDegrees(-80.0, 35.0),
      ellipse: {
        semiMinorAxis: 200000.0,
        semiMajorAxis: 500000.0,
        rotation: Cesium.Math.toRadians(30.0),
        material: stripeMaterial,
      },
    });

    entities.add({
      position: Cesium.Cartesian3.fromDegrees(-72.0, 35.0),
      ellipse: {
        semiMinorAxis: 200000.0,
        semiMajorAxis: 200000.0,
        rotation: Cesium.Math.toRadians(30.0),
        material: stripeMaterial,
      },
    });

    entities.add({
      rectangle: {
        coordinates: Cesium.Rectangle.fromDegrees(
            -110.0,
            38.0,
            -107.0,
            40.0
        ),
        height: 700000.0,
        extrudedHeight: 1000000.0,
        rotation: Cesium.Math.toRadians(45),
        material: Cesium.Color.fromRandom({ alpha: 1.0 }),
      },
    });

    entities.add({
      position: Cesium.Cartesian3.fromDegrees(-110.0, 35.0),
      ellipse: {
        semiMinorAxis: 100000.0,
        semiMajorAxis: 200000.0,
        height: 300000.0,
        extrudedHeight: 700000.0,
        rotation: Cesium.Math.toRadians(-40.0),
        material: Cesium.Color.fromRandom({ alpha: 1.0 }),
      },
    });

    entities.add({
      polygon: {
        hierarchy: new Cesium.PolygonHierarchy(
            Cesium.Cartesian3.fromDegreesArray([
              -113.0,
              30.0,
              -110.0,
              30.0,
              -110.0,
              33.0,
              -111.5,
              31.0,
              -113.0,
              33.0,
            ])
        ),
        extrudedHeight: 300000.0,
        material: Cesium.Color.fromRandom({ alpha: 1.0 }),
      },
    });

    entities.add({
      position: Cesium.Cartesian3.fromDegrees(-70.0, 40.0, 200000.0),
      cylinder: {
        hierarchy: new Cesium.PolygonHierarchy(
            Cesium.Cartesian3.fromDegreesArray([
              -118.0,
              30.0,
              -115.0,
              30.0,
              -117.1,
              31.1,
              -118.0,
              33.0,
            ])
        ),
        length: 400000.0,
        topRadius: 0.0,
        bottomRadius: 200000.0,
        material: Cesium.Color.fromRandom({ alpha: 1.0 }),
      },
    });

    for (i = 0; i < 5; ++i) {
      height = 200000.0 * i;

      entities.add({
        position: Cesium.Cartesian3.fromDegrees(-65.0, 35.0),
        ellipse: {
          semiMinorAxis: 200000.0,
          semiMajorAxis: 200000.0,
          height: height,
          material: Cesium.Color.fromRandom({ alpha: 0.5 }),
        },
      });

      entities.add({
        rectangle: {
          coordinates: Cesium.Rectangle.fromDegrees(
              -67.0,
              27.0,
              -63.0,
              32.0
          ),
          height: height,
          material: Cesium.Color.fromRandom({ alpha: 0.5 }),
        },
      });
    }

    for (i = 0; i < 5; ++i) {
      height = 100000.0 + 200000.0 * i;
      entities.add({
        position: Cesium.Cartesian3.fromDegrees(-108.0, 45.0, height),
        box: {
          dimensions: new Cesium.Cartesian3(90000.0, 90000.0, 90000.0),
          material: Cesium.Color.fromRandom({ alpha: 1.0 }),
        },
      });

      entities.add({
        position: Cesium.Cartesian3.fromDegrees(-104.0, 45.0, height),
        ellipsoid: {
          radii: new Cesium.Cartesian3(45000.0, 45000.0, 90000.0),
          material: Cesium.Color.fromRandom({ alpha: 1.0 }),
        },
      });

      entities.add({
        position: Cesium.Cartesian3.fromDegrees(-100.0, 45.0, height),
        ellipsoid: {
          radii: new Cesium.Cartesian3(67500.0, 67500.0, 67500.0),
          material: Cesium.Color.fromRandom({ alpha: 1.0 }),
        },
      });
    }

    positions = [];
    for (i = 0; i < 40; ++i) {
      positions.push(Cesium.Cartesian3.fromDegrees(-100.0 + i, 15.0));
    }

    entities.add({
      polyline: {
        positions: positions,
        width: 10.0,
        material: new Cesium.PolylineGlowMaterialProperty({
          color: Cesium.Color.DEEPSKYBLUE,
          glowPower: 0.25,
        }),
      },
    });

    positions = [];
    for (i = 0; i < 40; ++i) {
      positions.push(Cesium.Cartesian3.fromDegrees(-100.0 + i, 9.0));
    }

    entities.add({
      wall: {
        positions: Cesium.Cartesian3.fromDegreesArrayHeights([
          -90.0,
          43.0,
          100000.0,
          -87.5,
          45.0,
          100000.0,
          -85.0,
          43.0,
          100000.0,
          -87.5,
          41.0,
          100000.0,
          -90.0,
          43.0,
          100000.0,
        ]),
        material: new Cesium.CheckerboardMaterialProperty({
          repeat: new Cesium.Cartesian2(20.0, 6.0),
        }),
      },
    });

    entities.add({
      corridor: {
        positions: Cesium.Cartesian3.fromDegreesArray([
          -120.0,
          45.0,
          -125.0,
          50.0,
          -125.0,
          55.0,
        ]),
        width: 100000,
        material: Cesium.Color.fromRandom({ alpha: 1.0 }),
      },
    });

    entities.add({
      corridor: {
        positions: Cesium.Cartesian3.fromDegreesArray([
          -120.0,
          45.0,
          -125.0,
          50.0,
          -125.0,
          55.0,
        ]),
        width: 100000,
        height: 300000,
        extrudedHeight: 400000,
        material: Cesium.Color.fromRandom({ alpha: 0.7 }),
      },
    });

    entities.add({
      corridor: {
        positions: Cesium.Cartesian3.fromDegreesArray([
          -120.0,
          45.0,
          -125.0,
          50.0,
          -125.0,
          55.0,
        ]),
        width: 100000,
        height: 700000,
        outline: true,
        outlineColor: Cesium.Color.WHITE,
        outlineWidth: 4,
        material: Cesium.Color.fromRandom({ alpha: 0.7 }),
      },
    });

    function starPositions(arms, rOuter, rInner) {
      var angle = Math.PI / arms;
      var pos = [];
      for (var i = 0; i < 2 * arms; i++) {
        var r = i % 2 === 0 ? rOuter : rInner;
        var p = new Cesium.Cartesian2(
            Math.cos(i * angle) * r,
            Math.sin(i * angle) * r
        );
        pos.push(p);
      }
      return pos;
    }

    entities.add({
      polylineVolume: {
        positions: Cesium.Cartesian3.fromDegreesArrayHeights([
          -102.0,
          15.0,
          100000.0,
          -105.0,
          20.0,
          200000.0,
          -110.0,
          20.0,
          100000.0,
        ]),
        shape: starPositions(7, 30000.0, 20000.0),
        outline: true,
        outlineColor: Cesium.Color.WHITE,
        outlineWidth: 1,
        material: Cesium.Color.fromRandom({ alpha: 1.0 }),
      },
    });

    entities.add({
      polylineVolume: {
        positions: Cesium.Cartesian3.fromDegreesArray([
          -102.0,
          15.0,
          -105.0,
          20.0,
          -110.0,
          20.0,
        ]),
        shape: starPositions(7, 30000.0, 20000.0),
        material: Cesium.Color.fromRandom({ alpha: 1.0 }),
      },
    });

    function computeCircle(radius) {
      var positions = [];
      for (var i = 0; i < 360; i++) {
        var radians = Cesium.Math.toRadians(i);
        positions.push(
            new Cesium.Cartesian2(
                radius * Math.cos(radians),
                radius * Math.sin(radians)
            )
        );
      }
      return positions;
    }

    entities.add({
      polylineVolume: {
        positions: Cesium.Cartesian3.fromDegreesArray([
          -104.0,
          13.0,
          -107.0,
          18.0,
          -112.0,
          18.0,
        ]),
        shape: computeCircle(40000.0),
        material: Cesium.Color.WHITE,
      },
    });

    viewer.zoomTo(viewer.entities);


  }
}
</script>

<template>
  <div id="cesiumContainer"></div>
</template>

<style>

</style>
