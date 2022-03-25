<template>
    <div class="box">
        <div class="view" id='viewS'> </div>
        <div class="scan" id='scan' ></div>
    </div>
</template>


<script lang="ts">
    import { defineComponent,ref,onMounted, reactive } from 'vue'
    import OpenSeadragon from 'openseadragon';
    export default defineComponent({
        name: 'App',
        setup({ctx}){
            let mainView = ref();
            let view = reactive({});
            let scan = reactive({});
            const emitZoom = ()=>{
                scan.viewport.zoomTo(view.viewport.getZoom());
                scan.viewport.panTo(view.viewport.getCenter(), true);
            }
            const emitViewportChange = ()=>{

            }
            const canvasClick = (even:object)=>{
                if(even.quick){
                    let zoom = view.viewport.getZoom()
                    let newZoon = 0;
                    if(zoom < 2){
                        newZoon = 2;
                    } else if(zoom < 4){
                        newZoon = 4;
                    } else if(zoom<10){
                        newZoon = 10
                    } else if(zoom<20){
                        newZoon = 20
                    }else if(zoom<40){
                        newZoon = 40
                    }
                    if(even.shift) newZoon = 40;
                    const viewportPoint = view.viewport.pointFromPixel(even.position);
                    scan.viewport.zoomTo(newZoon);
                    scan.viewport.panTo(viewportPoint, true);
                }
            }

            const initView= ()=>{
                view = new OpenSeadragon({
                    element: document.getElementById('viewS'),
                    showZoomControl: false,
                    showHomeControl: false,
                    showFullPageControl: false,
                    // zoomPerClick: 0,
                    visibilityRatio: 1,
                    animationTime: 0.1,
                    tileSources: {
                        type: 'image',
                        url: 'https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fup.enterdesk.com%2Fphoto%2F2008-6-13%2F200806131108306030.jpg&refer=http%3A%2F%2Fup.enterdesk.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=auto?sec=1650766794&t=525766e5da070922ca2bdd0caf61e7dc',
                        // url: 'http://192.168.1.73/analysis.php?file=/media/hzzt/10C0C252C0C23DAA/LinuxData/scannerData/Stomach/GF2018-030338-2020-02-19_13_03_00_TEST_hotmap.jpg'
                    },
                    gestureSettingsMouse: {
                        zoomToRefPoint: true,
                        flickMinSpeed: 3,
                        flickMomentum: 3,
                    },
                    zoomPerClick: 1,
                    autoHideControls: false,
                    showNavigator: false,
                })
                // view.addHandler('tile-loaded', closeLoading);
                view.addHandler('canvas-scroll', emitZoom);
                view.addHandler('canvas-drag-end', emitViewportChange);
                view.addHandler('canvas-click', canvasClick)
                scan = new OpenSeadragon({
                    element: document.getElementById('scan'),
                    showZoomControl: false,
                    showHomeControl: false,
                    showFullPageControl: false,
                    // zoomPerClick: 0,
                    visibilityRatio: 1,
                    animationTime: 0.1,
                    tileSources: {
                        type: 'image',
                        url: 'https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fup.enterdesk.com%2Fphoto%2F2008-6-13%2F200806131108306030.jpg&refer=http%3A%2F%2Fup.enterdesk.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=auto?sec=1650766794&t=525766e5da070922ca2bdd0caf61e7dc',
                        // url: 'http://192.168.1.73/analysis.php?file=/media/hzzt/10C0C252C0C23DAA/LinuxData/scannerData/Stomach/GF2018-030338-2020-02-19_13_03_00_TEST_hotmap.jpg'
                    },
                    gestureSettingsMouse: {
                        zoomToRefPoint: true,
                        flickMinSpeed: 3,
                        flickMomentum: 3,
                    },
                    zoomPerClick: 1,
                    autoHideControls: false,
                    showNavigator: false,
                })
            }


            onMounted(()=>{
                initView();
                // window.addEventListener('load', function(){
                //     console.log('saaa');
                //
                //     Grade(document.querySelectorAll('.my-div'))
                // })
                // let n = 4,j=0;
                // let arr = [n];
                // let ans = [];
                // for(let i=0 ;i <= n ;i++){
                //     arr.push(--n)
                //     arr.push(n)
                // }
                // console.log(arr, 'arr');

            })
            return { mainView}
        }
    })
</script>

<style scoped>
    .box{
        display: flex;
        flex: 1;
    }
    .view {
        height: 400px;
        flex: 1;
        background: #00ffff;
    }
    .scan {
        height: 400px;
        flex: 1;
        background: bisque;
    }
</style>