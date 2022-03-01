<template>
  <div id="viewDiv"></div>
</template>

<script>
import esriConfig from "@arcgis/core/config";
import Map from "@arcgis/core/Map";
import MapView from "@arcgis/core/views/MapView";
import FeatureLayer from "@arcgis/core/layers/FeatureLayer";
import Bookmarks from "@arcgis/core/widgets/Bookmarks";
import Expand from "@arcgis/core/widgets/Expand";
import Legend from "@arcgis/core/widgets/Legend";
import Editor from "@arcgis/core/widgets/Editor";

export default {
  data() {
    return {
      /* 定义Key */
      key: {
        api_default: "AAPK362e52cac28d43e9a3475c8046cbd485iAi9gf8dE5U2QUdZoUEUQTQ8nWIRLrY3heX0V-oxgE5EszyF2JrK-pScTpH038d2"
      },
      layer: {
        feature: {
          point: "https://services7.arcgis.com/CcyYTolGT96wpo1a/arcgis/rest/services/FeatureLayer_FJCPC/FeatureServer/0",
          line: "https://services7.arcgis.com/CcyYTolGT96wpo1a/arcgis/rest/services/FeatureLayer_FJCPC/FeatureServer/1",
          polygon: "https://services7.arcgis.com/CcyYTolGT96wpo1a/arcgis/rest/services/FeatureLayer_FJCPC/FeatureServer/2"
        }
      },
      /* 定义模板 */
      template: {
        popup: {
          title: "{Name}",
          content: [{
            type: "fields",
            fieldInfos: [{
              fieldName: "esrignss_longitude",
              label: "经度"
            }, {
              fieldName: "esrignss_latitude",
              label: "纬度"
            }, {
              fieldName: "esrignss_altitude",
              label: "高度"
            }, {
              fieldName: "CreationDate",
              label: "创建时间"
            }, {
              fieldName: "Creator",
              label: "创建者"
            }, {
              fieldName: "EditDate",
              label: "编辑日期"
            }, {
              fieldName: "Editor",
              label: "编辑器"
            }, {
              fieldName: "esrignss_url",
              label: "链接"
            }, {
              fieldName: "esrignss_name",
              label: "名称"
            }]
          }]
        }
      },
    }
  },
  name: 'App',
  mounted: async function () {
    esriConfig.apiKey = this.key.api_default

    // 实例化要素图层对象（通过url获取ArcGIS云托管图层）
    const featureLayer = new FeatureLayer({
      title: "福建船政交通职业学院",
      copyright: "福建船政交通职业学院",
      url: this.layer.feature.point,
      popupTemplate: this.template.popup
    })

    // 实例化地图对象
    const map = new Map({
      basemap: "osm-streets",
      layers: [featureLayer]
    })

    // 实例化地图视图，作为Map对象的容器
    const mapView = new MapView({
      // https://v3.vuejs.org/api/instance-properties.html#el
      container: "viewDiv",
      center: [119.311998, 26.027573],
      zoom: 13,
    });
    mapView.map = map;

    // 实例化书签组件和Expand组件（Bookmarks对象和Expand对象）
    const bookmarks = new Bookmarks({
      view: mapView,
      // 允许添加、编辑或删除书签
      editingEnabled: true
    });

    const bkExpand = new Expand({
      view: mapView,
      content: bookmarks,
      expanded: true,
    });

    // 实例化Legend组件，用于展开&收起图例组件（Legend对象和Expand对象）
    const legend = new Legend({
      view: mapView,
    })

    const legendExpand = new Expand({
      view: mapView,
      content: legend,
      expand: true
    })

    // 实例化Editor组件，用于展开&收起图例组件（Editor对象和Expand）
    const editor = new Editor({
      view: mapView
    })

    const editorExpand = new Expand({
      view: mapView,
      content: editor,
      expand: false
    })

    // mapView 生命周期
    mapView.when(function () {
      // 将小部件添加到视图的右上角
      // 向视图添加书签
      mapView.ui.add(bkExpand, "top-right")
      // 向视图添加图例
      mapView.ui.add(legendExpand, "top-right")
      // 向视图添加编辑器
      mapView.ui.add(editorExpand, "top-right")

      if (mapView.bookmarks && mapView.bookmarks.length) {
        console.log("Bookmarks: ", mapView.bookmarks.length);
      } else {
        console.log("No bookmarks in this mapView.");
      }
    });
  },
  methods: {}
}
</script>

<style scoped>
@import './main.css';
</style>
