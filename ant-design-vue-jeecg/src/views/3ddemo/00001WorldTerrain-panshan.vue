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

    // Initialize the Cesium Viewer in the HTML element with the `cesiumContainer` ID.
    const viewer = new Cesium.Viewer('cesiumContainer', {
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
    viewer.scene.globe.depthTestAgainstTerrain = true;
    //去除版权信息
    viewer._cesiumWidget._creditContainer.style.display = "none";

    // Add Cesium OSM Buildings, a global 3D buildings layer.
    viewer.scene.primitives.add(Cesium.createOsmBuildings());
    // set lighting to true
    viewer.scene.globe.enableLighting = true;

    viewer.entities.add({
      position: Cesium.Cartesian3.fromDegrees(117.3392, 40.2958,1000),
      label: {
        text: "盘山",
        translucencyByDistance: new Cesium.NearFarScalar(
          1.5e5,
          1.0,
          1.5e7,
          0.0
        ),
      },
    });

    // Fly the camera to San Francisco at the given longitude, latitude, and height.
    viewer.camera.flyTo({
      destination: Cesium.Cartesian3.fromDegrees(117.289679, 40.108261, 2000),
      orientation: {
        heading: Cesium.Math.toRadians(15.0),
        pitch: Cesium.Math.toRadians(-15.0),
      }
    });

    const handler = new Cesium.ScreenSpaceEventHandler(viewer.scene.canvas);
    let arrays = []
    handler.setInputAction(function (event) {

      if(!Cesium.defined(event.position)){
        return
      }

      const p = viewer.scene.pickPosition(event.position);

      if(!Cesium.defined(p)){
        return
      }

      var cartographic = Cesium.Cartographic.fromCartesian(p);
      var lon = Cesium.Math.toDegrees(cartographic.longitude);
      var lat = Cesium.Math.toDegrees(cartographic.latitude);
      var h = viewer.scene.globe.getHeight(cartographic);

      // alert("lon="+lon+",lat="+lat+",h="+h)
      arrays.push(lon,lat)

    }, Cesium.ScreenSpaceEventType.LEFT_CLICK);

    handler.setInputAction(function (event) {
      console.log(arrays)
      arrays = []
    }, Cesium.ScreenSpaceEventType.RIGHT_CLICK);

    let border = [
      117.2976706840122,
      40.185672840507486,
      117.29658219209904,
      40.1843983530712,
      117.29700392983555,
      40.18236477394026,
      117.2978391498434,
      40.181900618403134,
      117.29768686629316,
      40.18054848166154,
      117.29766793295617,
      40.17888564388528,
      117.29686066461448,
      40.177628421745624,
      117.29881034541843,
      40.177107506357316,
      117.2985447996301,
      40.175773228575586,
      117.29871134658504,
      40.17390960866226,
      117.29952272282128,
      40.171659549506934,
      117.30025778715971,
      40.17038959848186,
      117.30213663153341,
      40.16815643645295,
      117.30430556375951,
      40.16894082595155,
      117.30611856967148,
      40.17130798739684,
      117.307150274354,
      40.169657113481406,
      117.30853709823789,
      40.16745043792593,
      117.30934012299423,
      40.16683476112147,
      117.30988420323696,
      40.16476172607277,
      117.31082248659227,
      40.16316044716363,
      117.31259331864423,
      40.16136400015359,
      117.31526428711148,
      40.158943523901776,
      117.31810966918704,
      40.1574643987829,
      117.31755847306486,
      40.156262689495335,
      117.32082967183963,
      40.15563773260321,
      117.32521949061413,
      40.156555927224936,
      117.32693460460759,
      40.157393148469694,
      117.32890749769646,
      40.15739027947253,
      117.33348667172213,
      40.156889359776926,
      117.33958946370237,
      40.156598216994695,
      117.34165234911595,
      40.15893551105245,
      117.34098543879094,
      40.16195677173529,
      117.33546041620411,
      40.16427768948376,
      117.33106742825323,
      40.165233270273795,
      117.32768776535578,
      40.16553473920584,
      117.32708131670418,
      40.167592940147216,
      117.32733465256032,
      40.16990138661629,
      117.32606500183937,
      40.17212183783709,
      117.31935137735402,
      40.171090970427194,
      117.31669815913705,
      40.1723130893997,
      117.31622238806281,
      40.17633492211906,
      117.31763718339542,
      40.17617398680928,
      117.31872955219998,
      40.17448833706267,
      117.32201758484092,
      40.173544009946056,
      117.32412637862728,
      40.17337218412315,
      117.32724638577123,
      40.1736886761396,
      117.32745226541556,
      40.1749105883692,
      117.3242166356018,
      40.175803549098745,
      117.32358056881064,
      40.17672588207529,
      117.32984481428252,
      40.176416447219964,
      117.33178085281143,
      40.177344305149866,
      117.33501780132354,
      40.177414131278596,
      117.33845687976616,
      40.177743137706095,
      117.34037310556526,
      40.17914778825907,
      117.34177789315149,
      40.18002656404458,
      117.33932820950061,
      40.18152763581801,
      117.33463858820546,
      40.181744103241165,
      117.33095398453308,
      40.181977559910884,
      117.32881520122439,
      40.182419580372176,
      117.32698273728128,
      40.183676272638,
      117.32508649423112,
      40.18519092637653,
      117.3218217821057,
      40.18582272728679,
      117.32065198836376,
      40.184572229133636,
      117.31677272288353,
      40.185167221834966,
      117.31273415034045,
      40.18563051999785,
      117.31230201177073,
      40.186343281524465,
      117.31150389588718,
      40.18775728567289,
      117.31156747160023,
      40.18934432520831,
      117.30840147211909,
      40.18896867359621,
      117.30744281185042,
      40.187903011939135,
      117.30174783626278,
      40.18724374936465,
      117.29798619474217,
      40.187259728278256,
      117.29546754482163,
      40.185175695522005,
      117.2951244203367,
      40.183093119322066,
      117.29676164400621,
      40.18187122106075
    ]
    // let border = [
    //   117.29427073116102,
    //   40.18748510112407,
    //   117.2933576150823,
    //   40.18313404862713,
    //   117.29396032047673,
    //   40.17753575687285,
    //   117.29572324094207,
    //   40.17342576396387,
    //   117.29771834303774,
    //   40.16853881043568,
    //   117.29974669019272,
    //   40.16480374166219,
    //   117.30794437176506,
    //   40.15934251173409,
    //   117.32383391008509,
    //   40.15464823778376,
    //   117.33503902126853,
    //   40.15384156155255,
    //   117.3422452552694,
    //   40.15561333662203,
    //   117.34365296738514,
    //   40.16000057741816,
    //   117.34378802067756,
    //   40.16385754250992,
    //   117.33638805512993,
    //   40.164991082732755,
    //   117.332432246366,
    //   40.16708761404411,
    //   117.32996435364413,
    //   40.16903876347105,
    //   117.32502990051013,
    //   40.17252605406252,
    //   117.33195669117175,
    //   40.17369078280581,
    //   117.33960944418992,
    //   40.175919131371856,
    //   117.3434523738324,
    //   40.17809763582961,
    //   117.34394284723986,
    //   40.181748028357916,
    //   117.33347559059003,
    //   40.18477539180434,
    //   117.31867127333032,
    //   40.18845742143487,
    //   117.3106127188361,
    //   40.189791226284335,
    //   117.2982957166717,
    //   40.18990391689288,
    //   117.29390222301377,
    //   40.18635111728913,
    //   117.29362052321895,
    //   40.178048145849836,
    //   117.29374862133074,
    //   40.17484925107457,
    //   117.29601022615044,
    //   40.173472143621495
    // ]

    //_polygonArr 为polygon的坐标
    let waterPrimitive = new Cesium.Primitive({
      allowPicking: false,
      geometryInstances: new Cesium.GeometryInstance({
        geometry: new Cesium.PolygonGeometry({
          //polygonHierarchy: new Cesium.PolygonHierarchy(Cesium.Cartesian3.fromDegreesArrayHeights(border)),
          polygonHierarchy: new Cesium.PolygonHierarchy(Cesium.Cartesian3.fromDegreesArray(border)),
          // rectangle:Cesium.Rectangle.fromDegrees(-180, -90, 180.0, 90.0),
          vertexFormat: Cesium.EllipsoidSurfaceAppearance.VERTEX_FORMAT,
          // extrudedHeight: 0,
          height: 105,
          perPositionHeight: false
          // perPositionHeight : true//注释掉此属性水面就贴地了
        })
        //通过extrudedHeight属性控制的是这个水面的高度也就是厚度，而height控制的是水体整个距离地面抬高的高度。
      }),

      // 使用内置的水面shader
      appearance: new Cesium.EllipsoidSurfaceAppearance({
        aboveGround: true,
        material: new Cesium.Material({
          fabric: {
            type: 'Water',
            uniforms: {
              blendColor: new Cesium.Color(0.0, 0.0, 1.0, 0.2),
              //设置水面使用的图片，
              //此图片在Cesium源码Source\Assets\Textures文件夹中
              normalMap: '/models/waterNormals.jpeg',
              //频率速度设置
              frequency: 100.0,
              animationSpeed: 0.01,
              amplitude: 3.0
            }
          }
        })
        //frameShader代码也可以根据需要进行修改
        //fragmentShaderSource: 'varying vec3 v_positionMC;
        //vec3 v_positionEC;vec2 v_st;\nvoid main()\n{\nczm_materialInput materialInput;\nvec3 normalEC = normalize(czm_normal3D * czm_geodeticSurfaceNormal(v_positionMC, vec3(0.0), vec3(1.0)));\n#ifdef FACE_FORWARD\nnormalEC = faceforward(normalEC, vec3(0.0, 0.0, 1.0), -normalEC);\n#endif\nmaterialInput.s = v_st.s;\nmaterialInput.st = v_st;\nmaterialInput.str = vec3(v_st, 0.0);\nmaterialInput.normalEC = normalEC;\nmaterialInput.tangentToEyeMatrix = czm_eastNorthUpToEyeCoordinates(v_positionMC, materialInput.normalEC);\nvec3 positionToEyeEC = -v_positionEC;\nmaterialInput.positionToEyeEC = positionToEyeEC;\nczm_material material = czm_getMaterial(materialInput);\n#ifdef FLAT\ngl_FragColor = vec4(material.diffuse + material.emission, material.alpha);\n#else\ngl_FragColor = czm_phong(normalize(positionToEyeEC), material);\
        // gl_FragColor.a=0.5;\n#endif\n}\n' //重写shader，修改水面的透明度
      })
    });
    //添加水面数据到viewer中
    viewer.scene.primitives.add(waterPrimitive);
  }
}
</script>

<template>
  <div id="cesiumContainer"></div>
</template>

<style>

</style>
