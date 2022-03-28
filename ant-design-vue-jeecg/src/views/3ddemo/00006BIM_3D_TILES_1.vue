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
          url: 'http://3dcloud.365power.cn/files/waters/model/building/tileset.json'
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
    attachTileset(viewer, tileset);
  }
}

function attachTileset(viewer, tileset) {
  var picking = true;
  var dbIdToFeatures = {};
  var hiddenDbIds = [];

  var selected = [];
  var selectedDbId = -1;
  var highlighted = [];
  var highlightedDbId = -1;

  var leftDown = false;
  var middleDown = false;
  var rightDown = false;
  var pinchStart = false;

  var selectedEntity = new Cesium.Entity();

  tileset.colorBlendMode = Cesium.Cesium3DTileColorBlendMode.REPLACE;

  var handler = new Cesium.ScreenSpaceEventHandler(viewer.scene.canvas);
  handler.setInputAction(function onMouseMove(movement) {
    if (!picking) return;
    if (leftDown || middleDown || rightDown || pinchStart) return;

    var pickedFeature = viewer.scene.pick(movement.endPosition);
    if (Cesium.defined(pickedFeature) && pickedFeature instanceof Cesium.Cesium3DTileFeature && pickedFeature.tileset === tileset) {
      var dbId = getFeatureDbId(pickedFeature);
      setHighlighted(dbId);
    } else {
      clearHighlighted();
    }
  }, Cesium.ScreenSpaceEventType.MOUSE_MOVE);

  handler.setInputAction(function onLeftClick(movement) {
    if (!picking) return;

    var pickedFeature = viewer.scene.pick(movement.position);
    if (Cesium.defined(pickedFeature) && pickedFeature instanceof Cesium.Cesium3DTileFeature && pickedFeature.tileset === tileset) {
      var dbId = getFeatureDbId(pickedFeature);
      if (dbId === selectedDbId) {
        clearSelected();
      } else {
        setSelected(dbId);
      }
    } else {
      clearSelected();
    }
  }, Cesium.ScreenSpaceEventType.LEFT_CLICK);

  handler.setInputAction(function onLeftClick(movement) {
    if (!picking) return;

    var pickedFeature = viewer.scene.pick(movement.position);
    if (Cesium.defined(pickedFeature) && pickedFeature instanceof Cesium.Cesium3DTileFeature && pickedFeature.tileset === tileset) {

      var minX = pickedFeature.getProperty('MinX');
      var minY = pickedFeature.getProperty('MinY');
      var minZ = pickedFeature.getProperty('MinZ');
      var maxX = pickedFeature.getProperty('MaxX');
      var maxY = pickedFeature.getProperty('MaxY');
      var maxZ = pickedFeature.getProperty('MaxZ');

      var sphere = Cesium.BoundingSphere.transform(Cesium.BoundingSphere.fromCornerPoints(new Cesium.Cartesian3(minX, minY, minZ), new Cesium.Cartesian3(maxX, maxY, maxZ)), tileset.root.computedTransform, new Cesium.BoundingSphere());
      var camera = viewer.scene.camera;
      var offset = new Cesium.HeadingPitchRange(camera.heading, camera.pitch, 0);
      camera.flyToBoundingSphere(sphere, {
        offset: offset
      });
    } else {
      clearSelected();
    }
  }, Cesium.ScreenSpaceEventType.RIGHT_CLICK);

  handler.setInputAction(function () {
    leftDown = true;
  }, Cesium.ScreenSpaceEventType.LEFT_DOWN);

  handler.setInputAction(function () {
    leftDown = false;
  }, Cesium.ScreenSpaceEventType.LEFT_UP);

  handler.setInputAction(function () {
    middleDown = true;
  }, Cesium.ScreenSpaceEventType.MIDDLE_DOWN);

  handler.setInputAction(function () {
    middleDown = false;
  }, Cesium.ScreenSpaceEventType.MIDDLE_UP);

  handler.setInputAction(function () {
    rightDown = true;
  }, Cesium.ScreenSpaceEventType.RIGHT_DOWN);

  handler.setInputAction(function () {
    rightDown = false;
  }, Cesium.ScreenSpaceEventType.RIGHT_UP);

  handler.setInputAction(function () {
    pinchStart = true;
  }, Cesium.ScreenSpaceEventType.PINCH_START);

  handler.setInputAction(function () {
    pinchStart = false;
  }, Cesium.ScreenSpaceEventType.PINCH_END);

  function getFeatureDbId(feature) {
    if (Cesium.defined(feature) && Cesium.defined(feature.getProperty)) {
      return parseInt(feature.getProperty('DbId'), 10);
    }
    return -1;
  }

  function setHighlighted(dbId) {

    if (dbId === highlightedDbId) return;

    clearHighlighted();
    highlightedDbId = dbId;

    if (highlightedDbId === selectedDbId || highlightedDbId < 0) {
      return;
    }

    var targetColor = Cesium.Color.YELLOW;
    var features = dbIdToFeatures[dbId];
    var _iteratorNormalCompletion = true;
    var _didIteratorError = false;
    var _iteratorError = undefined;

    try {
      for (var _iterator = features[Symbol.iterator](), _step; !(_iteratorNormalCompletion = (_step = _iterator.next()).done); _iteratorNormalCompletion = true) {
        var feature = _step.value;

        highlighted.push({
          feature: feature,
          originalColor: Cesium.Color.clone(feature.color)
        });
        feature.color = Cesium.Color.clone(targetColor, feature.color);
      }
    } catch (err) {
      _didIteratorError = true;
      _iteratorError = err;
    } finally {
      try {
        if (!_iteratorNormalCompletion && _iterator.return) {
          _iterator.return();
        }
      } finally {
        if (_didIteratorError) {
          throw _iteratorError;
        }
      }
    }
  }

  function setSelected(dbId) {

    if (dbId === selectedDbId) return;

    if (dbId === highlightedDbId) {
      clearHighlighted();
    }

    clearSelected();
    selectedDbId = dbId;

    if (selectedDbId < 0) {
      return;
    }

    console.log('Selected dbId: ' + dbId);

    selectedEntity.name = 'Load info for node ' + dbId;
    selectedEntity.description = 'Loading <div class="cesium-infoBox-loading"></div>';
    viewer.selectedEntity = selectedEntity;

    // var lastIndex = tileset.url.lastIndexOf('/');
    // var basePath = lastIndex == -1 ? "." : tileset.url.substr(0, lastIndex);

    fetch( 'http://3dcloud.365power.cn/files/waters/model/building/info/' + parseInt(dbId / 100) + '.json').then(function (response) {
      return response.json();
    }).then(function (json) {
      if (selectedDbId !== dbId) return;

      var node = json.data[dbId + ''];

      selectedEntity.name = node.name || "<null>";

      var strings = [];
      strings.push('<table class="cesium-infoBox-defaultTable"><tbody>');

      var _iteratorNormalCompletion2 = true;
      var _didIteratorError2 = false;
      var _iteratorError2 = undefined;

      try {
        for (var _iterator2 = node.categories[Symbol.iterator](), _step2; !(_iteratorNormalCompletion2 = (_step2 = _iterator2.next()).done); _iteratorNormalCompletion2 = true) {
          var category = _step2.value;

          var props = category.props;
          var propCount = category.count;
          var haveTitle = false;
          for (var i = 0; i < propCount; i++) {
            if (props.flags[i]) continue;

            if (!haveTitle) {
              haveTitle = true;
              strings.push('<tr><th colspan=2>' + category.name + '</th></tr>');
            }

            var value = props.values[i];
            switch (props.types[i]) {
              case 'boolean':
                value = value ? 'Yes' : 'No';
                break;
              case 'double':
                value = props.units[i] ? value.toFixed(3) + ' ' + props.units[i] : '' + value.toFixed(3);
                break;
              default:
                value = value + '';
                break;
            }

            strings.push('<tr><th>' + props.names[i] + '</th><td>' + value + '</td></tr>');
          }
        }
      } catch (err) {
        _didIteratorError2 = true;
        _iteratorError2 = err;
      } finally {
        try {
          if (!_iteratorNormalCompletion2 && _iterator2.return) {
            _iterator2.return();
          }
        } finally {
          if (_didIteratorError2) {
            throw _iteratorError2;
          }
        }
      }

      strings.push('</tbody></table>');

      selectedEntity.description = strings.join('');
    }).catch(function (err) {
      if (selectedDbId !== dbId) return;

      selectedEntity.description = err;
    });

    var targetColor = Cesium.Color.LIME;
    var features = dbIdToFeatures[dbId];
    var _iteratorNormalCompletion3 = true;
    var _didIteratorError3 = false;
    var _iteratorError3 = undefined;

    try {
      for (var _iterator3 = features[Symbol.iterator](), _step3; !(_iteratorNormalCompletion3 = (_step3 = _iterator3.next()).done); _iteratorNormalCompletion3 = true) {
        var feature = _step3.value;

        selected.push({
          feature: feature,
          originalColor: Cesium.Color.clone(feature.color)
        });
        feature.color = Cesium.Color.clone(targetColor, feature.color);
      }
    } catch (err) {
      _didIteratorError3 = true;
      _iteratorError3 = err;
    } finally {
      try {
        if (!_iteratorNormalCompletion3 && _iterator3.return) {
          _iterator3.return();
        }
      } finally {
        if (_didIteratorError3) {
          throw _iteratorError3;
        }
      }
    }
  }

  function clearHighlighted() {
    if (highlightedDbId < 0) return;

    if (highlighted.length > 0) {
      var _iteratorNormalCompletion4 = true;
      var _didIteratorError4 = false;
      var _iteratorError4 = undefined;

      try {
        for (var _iterator4 = highlighted[Symbol.iterator](), _step4; !(_iteratorNormalCompletion4 = (_step4 = _iterator4.next()).done); _iteratorNormalCompletion4 = true) {
          var item = _step4.value;

          item.feature.color = item.originalColor;
        }
      } catch (err) {
        _didIteratorError4 = true;
        _iteratorError4 = err;
      } finally {
        try {
          if (!_iteratorNormalCompletion4 && _iterator4.return) {
            _iterator4.return();
          }
        } finally {
          if (_didIteratorError4) {
            throw _iteratorError4;
          }
        }
      }

      highlighted = [];
    }

    highlightedDbId = -1;
  }

  function clearSelected() {
    if (selected.length > 0) {
      var _iteratorNormalCompletion5 = true;
      var _didIteratorError5 = false;
      var _iteratorError5 = undefined;

      try {
        for (var _iterator5 = selected[Symbol.iterator](), _step5; !(_iteratorNormalCompletion5 = (_step5 = _iterator5.next()).done); _iteratorNormalCompletion5 = true) {
          var item = _step5.value;

          item.feature.color = item.originalColor;
        }
      } catch (err) {
        _didIteratorError5 = true;
        _iteratorError5 = err;
      } finally {
        try {
          if (!_iteratorNormalCompletion5 && _iterator5.return) {
            _iterator5.return();
          }
        } finally {
          if (_didIteratorError5) {
            throw _iteratorError5;
          }
        }
      }

      selected = [];
    }

    selectedDbId = -1;

    if (viewer.selectedEntity === selectedEntity) {
      viewer.selectedEntity = null;
    }
  }

  function unloadFeature(feature) {
    var dbId = getFeatureDbId(feature);
    var features = dbIdToFeatures[dbId];
    features.splice(features.findIndex(function (item) {
      return item.feature === feature;
    }), 1);

    if (dbId === selectedDbId) {
      selected.splice(selected.findIndex(function (item) {
        return item.feature === feature;
      }), 1);
    }

    if (dbId === highlighted) {
      highlighted.splice(highlighted.findIndex(function (item) {
        return item.feature === feature;
      }), 1);
    }
  }

  function loadFeature(feature) {
    var dbId = getFeatureDbId(feature);
    var features = dbIdToFeatures[dbId];
    if (!Cesium.defined(features)) {
      dbIdToFeatures[dbId] = features = [];
    }
    features.push(feature);

    if (hiddenDbIds.indexOf(dbId) > -1) {
      feature.show = false;
    }
  }

  function processContentFeatures(content, callback) {
    var featuresLength = content.featuresLength;
    for (var i = 0; i < featuresLength; ++i) {
      var feature = content.getFeature(i);
      callback(feature);
    }
  }

  function processTileFeatures(tile, callback) {
    var content = tile.content;
    var innerContents = content.innerContents;
    if (Cesium.defined(innerContents)) {
      var length = innerContents.length;
      for (var i = 0; i < length; ++i) {
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
}

</script>

<template>
  <div id="cesiumContainer"></div>
</template>

<style>

</style>
