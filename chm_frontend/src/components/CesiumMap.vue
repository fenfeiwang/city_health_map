<template>
    <div id="cesiumContainer"></div>
</template>

<script>
import http from 'vue-resource';
import Vue from 'vue';
Vue.use(http);

export default {
    name: 'CesiumMap',
    mounted() {
        let Cesium = window.cesium;
        Cesium.Ion.defaultAccessToken = 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJqdGkiOiI0ODQ1MGEwOS1mMDk4LTQ4NDMtYTRkYi01Mjc1NDI3MWQ2ZGMiLCJpZCI6ODkxNiwic2NvcGVzIjpbImFzciIsImdjIl0sImlhdCI6MTU1MzA1NDMzMH0.Fndg4WDUnaD1rXyfNvsi3UDeJFQ7g9dR5U_owEhpLRw';
        var viewer = new Cesium.Viewer('cesiumContainer');
        viewer.imageryLayers.addImageryProvider(new Cesium.BingMapsImageryProvider({
            url : 'https://dev.virtualearth.net',
            key : 'AoP-LhJzZplGyzyekYwMdZ_U8V_J-6Cr2fRvOoQjU3lbNMApYha5J9P6D-7Kc-eY',
            mapStyle : Cesium.BingMapsStyle.CANVAS_LIGHT
        })); //Cesium.BingMapsStyle.CANVAS_GRAY  Cesium.BingMapsStyle.CANVAS_LIGHT Cesium.BingMapsStyle.CANVAS_DARK
        viewer.camera.setView({
            destination: Cesium.Cartesian3.fromDegrees(116.3974, 39.9342, 20000)
        });
        // this.addPoint(Cesium.Cartesian3.fromDegrees(116.2895356,40.00400583),viewer,Cesium);
        this.$http.get(`${process.env.BASE_URL}gps_date_time_lon_lat.csv`)
        .then(response=>{
            let gpsObj = this.csv2json(response.body);
            let a = 0;
            gpsObj.forEach(data=>{
                if(data && data.Longitude_NTU && data.Latitude_NTU){
                    this.addPoint(Cesium.Cartesian3.fromDegrees(data.lon,data.lat),viewer,Cesium.Color.FUCHSIA);
                }
                a++;
             })
        })
//        viewer.zoomTo(a);
    },
    methods:{
        addPoint(position,viewer,color) {
            return viewer.entities.add({
                point:{
                    color:color,
                    pixelSize:4,
                    show:true
                },
                position:position
            });
        },
        csv2json(csv) {
            var lines=csv.replace(/\"/g,"").split("\n");
            var result = [];
            var headers=lines[0].split(",");
            
            for(var i=1;i<lines.length;i++){
                var obj = {};
                var currentline=lines[i].split(",");
                for(var j=1;j<headers.length-1;j++){
                    obj[headers[j]] = currentline[j];
                }
                obj["Latitude"] = currentline[headers.length-1];
                obj["lat"] = this.gps2lonlat(obj["Latitude_NTU"])+0.001410833;
                obj["lon"] = this.gps2lonlat(obj["Longitude_NTU"])+0.006158333;
                result.push(obj);
            }
            console.log(result[0]);
            return result;
        },
        gps2lonlat(num) {
            let i = num/100;
            let a = Math.floor(i);
            let b = i - Math.floor(i);
            return a + b*100/60;
        }
    }
}
</script>

<style scoped>
.viewer {
  width: 100%;
  height: 100%;
}
</style>