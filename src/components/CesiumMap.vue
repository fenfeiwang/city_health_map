<template>
    <div id="cesiumContainer"></div>
</template>

<script>


export default {
    name: 'CesiumMap',
    mounted() {
        let Cesium = window.cesium;
        Cesium.Ion.defaultAccessToken = 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJqdGkiOiI0ODQ1MGEwOS1mMDk4LTQ4NDMtYTRkYi01Mjc1NDI3MWQ2ZGMiLCJpZCI6ODkxNiwic2NvcGVzIjpbImFzciIsImdjIl0sImlhdCI6MTU1MzA1NDMzMH0.Fndg4WDUnaD1rXyfNvsi3UDeJFQ7g9dR5U_owEhpLRw';
        var viewer = new Cesium.Viewer('cesiumContainer');
        viewer.imageryLayers.addImageryProvider(new Cesium.BingMapsImageryProvider({
            url : 'https://dev.virtualearth.net',
            key : 'AoP-LhJzZplGyzyekYwMdZ_U8V_J-6Cr2fRvOoQjU3lbNMApYha5J9P6D-7Kc-eY',
            mapStyle : Cesium.BingMapsStyle.CANVAS_GRAY
        }));
        viewer.camera.setView({
            destination: Cesium.Cartesian3.fromDegrees(116.3974, 39.9342, 20000)
        });
        let a = this.addPoint(Cesium.Cartesian3.fromDegrees(116.2895356,40.00400583),viewer,Cesium);
//        viewer.zoomTo(a);
    },
    methods:{
        addPoint(position,viewer,Cesium) {
            return viewer.entities.add({
                point:{
                    color:Cesium.Color.FUCHSIA,
                    pixelSize:4,
                    show:true
                },
                position:position
            });
        }
    }
}
</script>

<style scoped>
.viewer {
  width: 100%;
  height: 100%;
}
<style>