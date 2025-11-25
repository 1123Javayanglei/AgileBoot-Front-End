<script lang="ts" setup>
import { onMounted, onUnmounted } from "vue";
import AMapLoader from "@amap/amap-jsapi-loader";

let map = null;

onMounted(() => {
  AMapLoader.load({
    key: "2d1a1028032b59394d570aa1785a228a", // 申请好的Web端开发者Key，首次调用 load 时必填
    version: "2.0", // 指定要加载的 JSAPI 的版本，缺省时默认为 1.4.15
    plugins: [] // 需要使用的的插件列表，如比例尺'AMap.Scale'等
  })
    .then(AMap => {
      map = new AMap.Map("container", {
        // 设置地图容器id
        viewMode: "3D", // 是否为3D地图模式
        zoom: 16, // 初始化地图级别
        center: [118.930309, 31.981617] // 初始化地图中心点位置
      });

      AMap.plugin("AMap.Geolocation", function () {
        const geolocation = new AMap.Geolocation({
          enableHighAccuracy: true, //是否使用高精度定位，默认:true
          timeout: 10000, //超过10秒后停止定位，默认：5s
          position: "RB", //定位按钮的停靠位置
          offset: [10, 20], //定位按钮与设置的停靠位置的偏移量，默认：[10, 20]
          zoomToAccuracy: true //定位成功后是否自动调整地图视野到定位点q
        });
        map.addControl(geolocation);
      });
    })
    .catch(e => {
      console.log(e);
    });
});

onUnmounted(() => {
  map?.destroy();
});
</script>

<template>
  <div id="container" />
</template>

<style scoped>
#container {
  width: 100%;
  height: 100%;
}
</style>
