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
    let old_token = Cesium.Ion.defaultAccessToken
    Cesium.Ion.defaultAccessToken = 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJqdGkiOiI3ODZkMDQzOS03ZGJjLTQzZWUtYjlmYy04ZmM5Y2UwNzNhMmYiLCJpZCI6MjU5LCJpYXQiOjE2MzgyMDYwMDB9.cK1hsaFBgz0l2dG9Ry5vBFHWp-HF2lwjLC0tcK8Z8tY'
    // Power Plant design model provided by Bentley Systems
    const viewer = new Cesium.Viewer("cesiumContainer");
    const scene = viewer.scene;

    const tileset = scene.primitives.add(
      new Cesium.Cesium3DTileset({
        url: Cesium.IonResource.fromAssetId(8564),
      })
    );

    tileset.readyPromise
      .then(function (tileset) {
        viewer.zoomTo(
          tileset,
          new Cesium.HeadingPitchRange(
            0.5,
            -0.2,
            tileset.boundingSphere.radius * 4.0
          )
        );
      })
      .otherwise(function (error) {
        console.log(error);
      });

    tileset.colorBlendMode = Cesium.Cesium3DTileColorBlendMode.REPLACE;

    let selectedFeature;
    let picking = false;


    function selectFeature(feature) {
      const element = feature.getProperty("element");
      setElementColor(element, Cesium.Color.YELLOW);
      selectedFeature = feature;
    }

    function unselectFeature(feature) {
      if (!Cesium.defined(feature)) {
        return;
      }
      const element = feature.getProperty("element");
      setElementColor(element, Cesium.Color.WHITE);
      if (feature === selectedFeature) {
        selectedFeature = undefined;
      }
    }

    const handler = new Cesium.ScreenSpaceEventHandler(scene.canvas);
    handler.setInputAction(function (movement) {
      if (!picking) {
        return;
      }

      const feature = scene.pick(movement.endPosition);

      unselectFeature(selectedFeature);

      if (feature instanceof Cesium.Cesium3DTileFeature) {
        selectFeature(feature);
      }
    }, Cesium.ScreenSpaceEventType.MOUSE_MOVE);

// In this tileset every feature has an "element" property which is a global ID.
// This property is used to associate features across different tiles and LODs.
// Workaround until 3D Tiles has the concept of global batch ids: https://github.com/CesiumGS/3d-tiles/issues/265
    const elementMap = {}; // Build a map of elements to features.
    const hiddenElements = [
      112001,
      113180,
      131136,
      113167,
      71309,
      109652,
      111178,
      113156,
      113170,
      124846,
      114076,
      131122,
      113179,
      114325,
      131134,
      113164,
      113153,
      113179,
      109656,
      114095,
      114093,
      39225,
      39267,
      113149,
      113071,
      112003,
      39229,
      113160,
      39227,
      39234,
      113985,
      39230,
      112004,
      39223,
    ];

    function getElement(feature) {
      return parseInt(feature.getProperty("element"), 10);
    }

    function setElementColor(element, color) {
      const featuresToColor = elementMap[element];
      const length = featuresToColor.length;
      for (let i = 0; i < length; ++i) {
        const feature = featuresToColor[i];
        feature.color = Cesium.Color.clone(color, feature.color);
      }
    }

    function unloadFeature(feature) {
      unselectFeature(feature);
      const element = getElement(feature);
      const features = elementMap[element];
      const index = features.indexOf(feature);
      if (index > -1) {
        features.splice(index, 1);
      }
    }

    function loadFeature(feature) {
      const element = getElement(feature);
      let features = elementMap[element];
      if (!Cesium.defined(features)) {
        features = [];
        elementMap[element] = features;
      }
      features.push(feature);

      if (hiddenElements.indexOf(element) > -1) {
        feature.show = false;
      }
    }

    function processContentFeatures(content, callback) {
      const featuresLength = content.featuresLength;
      for (let i = 0; i < featuresLength; ++i) {
        const feature = content.getFeature(i);
        callback(feature);
      }
    }

    function processTileFeatures(tile, callback) {
      const content = tile.content;
      const innerContents = content.innerContents;
      if (Cesium.defined(innerContents)) {
        const length = innerContents.length;
        for (let i = 0; i < length; ++i) {
          processContentFeatures(innerContents[i], callback);
        }
      } else {
        processContentFeatures(content, callback);
      }
    }

    tileset.tileLoad.addEventListener(function (tile) {
      processTileFeatures(tile, loadFeature);
    });

    tileset.tileUnload.addEventListener(function (tile) {
      processTileFeatures(tile, unloadFeature);
    });


    Cesium.Ion.defaultAccessToken = old_token
  }
}
</script>

<template>
  <div id="cesiumContainer"></div>
</template>

<style>

</style>
