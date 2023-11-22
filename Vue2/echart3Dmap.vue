<template>
    <div class="container">
       <div id="map"></div>
    </div>
 </template>
 
 
 <script>
 import 'echarts-gl'
 import mapDot from '@/assets/icons/cockpit/map_dot.png'
 import geoJson from '@/assets/json/zj.json'
 export default {
   data () {
     return {
 
     }
   },
   mounted(){
     this.chartMap()
   },
   methods:{
     chartMap() {
       let echarts = this.$echarts
       var myChart = echarts.init(document.getElementById('map'))
       echarts.registerMap('浙江', geoJson)
       myChart.hideLoading()
       // 图表配置项
       let option = {
         geo3D: {
           map: '浙江',
           roam: true, //鼠标缩放平移功能
           // regionHeight:3,//地图厚度
           shading: 'realistic', //设定为color时 地图纹理才有效
           realisticMaterial: {
             // detailTexture: layoutMap, // 纹理贴图 格式支持（string, HTMLImageElement, HTMLCanvasElement）
             textureTiling: 1, // 纹理平铺，1是拉伸，数字表示纹理平铺次数
           },
           itemStyle: {
             // color: '#0052e9',
             color:"#005b12",//背景色
             opacity: 0.8,
             borderWidth: 0.8,
             borderColor: 'white',
           },
           viewControl: {
             projection: 'perspective',
             autoRotate: true,
             autoRotateDirection: 'cw', //物体自传方向
             autoRotateSpeed: 6,
             autoRotateAfterStill: 2, //鼠标静止多久后自动旋转
             distance: 150, //默认视角距离主体的距离(高度)
             minBeta: -Infinity,
             maxBeta: Infinity,
             // minAlpha: 5, // 上下旋转的最小 alpha 值。即视角能旋转到达最上面的角度。[ default: 5 ]
             // maxAlpha: 90, // 上下旋转的最大 alpha 值。即视角能旋转到达最下面的角度。[ default: 90 ]
             // minBeta: -3600, // 左右旋转的最小 beta 值。即视角能旋转到达最左的角度。[ default: -80 ]
             // maxBeta: 3600, // 左右旋转的最大 beta 值。即视角能旋转到达最右的角度。[ default: 80 ]
             animation: true, // 是否开启动画。[ default: true ]
             // animationDurationUpdate: 1000, // 过渡动画的时长。[ default: 1000 ]
             // animationEasingUpdate: "cubicInOut", // 过渡动画的缓动效果。[ default: cubicInOut ]
           },
           emphasis: {
             disabled: true, //是否可以被选中
             label: {
               show: true,
               formatter(params) {
                 // return `${params.data.name}`
                 return `{num|98}\n {block|} {pic|} {block|} \n{name|${params.name}}`
               },
               rich: {
                 pic: {
                   backgroundColor: {
                     image: mapDot,
                   },
                   width: 30,
                   height: 30,
                   align: 'center',
                 },
                 num: {
                   fontSize: 22,
                   fontWeight: 'bolder',
                   with: 100,
                   backgroundColor: '#e15d00',
                   color: 'white',
                   borderRadius: [15, 15, 15, 15],
                   padding: [10, 10, 5, 10],
                   align: 'center',
                   position: 'center',
                   shadowBlur: 1,
                   shadowColor: 'yellow',
                   shadowOffsetY: 5,
                 },
                 name: {
                   fontSize: 19,
                   lineHeight: 16,
                   color: 'yellow',
                   fontWeight: 'bolder',
                 },
                 block: {
                   height: 40,
                 },
               },
             },
             itemStyle: {
               color: '#e15d00', //显示移入的区块变色
             },
           },
           // 地图文字标签
           label: {
             show: true,
             formatter(params) {
               return ` {pic|} {block|} \n{name|${params.name}}`
               // return `{num|98}\n {block|} {pic|} {block|} \n{name|${params.name}}`
             },
             rich: {
               pic: {
                 backgroundColor: {
                   image: mapDot,
                 },
                 width: 30,
                 height: 30,
                 align: 'center',
                 borderWidth: 5,
               },
               num: {
                 fontSize: 20,
                 fontWeight: 'bolder',
                 with: 100,
                 backgroundColor: 'rgba(0, 0, 0, 0.74)',
                 color: '#fff',
                 borderRadius: [15, 15, 15, 15],
                 padding: [10, 10, 5, 10],
                 align: 'center',
                 position: 'top',
                 shadowBlur: 1,
                 shadowColor: 'rgba(0, 55, 255, 1)',
                 shadowOffsetY: 5,
                 offset: [0, -10],
                 top: -10,
               },
               name: {
                 fontSize: 17,
                 lineHeight: 16,
                 color: '#fff',
                 fontWeight: 'bolder',
               },
               block: {
                 height: 40,
               },
             },
           },
           // 在shading:'color'时候无效
           light: {
             //光照阴影
             main: {
               // color: "#fff", //光照颜色
               intensity: 1, //光照强度
               //shadowQuality: 'high', //阴影亮度
               shadow: true, //是否显示阴影
               shadowQuality: 'medium', //阴影质量 ultra //阴影亮度
               alpha: 125,
               beta: 20,
             },
             ambient: {
               intensity: 1,
             },
           },
           // 特效处理 环境光等
           postEffect: {
             enable: false,
             bloomIntensity: 0.5,
           },
         },
       }
       myChart.setOption(option)
       window.onresize = myChart.resize
     },
   }
 }
 </script>
 
 
 <style lang='scss' scoped>
 .container{
   width: 100vw;
   height: 100vh;
   #map{
     width: 100%;
     height: 80%;
   }
 }
 </style>
 