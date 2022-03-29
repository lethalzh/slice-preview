<template>
  <div class="domeTwo">
      <h2>domeTwo（左右同步）</h2>
      <div class="box">
          <div class="view" id='domeTwoView'></div>
          <div class="scan" id='domeTwoScan' ></div>
      </div>
  </div>
</template>

<script>
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
            const emitViewportChange = (even)=>{
                scan.viewport.panTo(view.viewport.getCenter(), true);
                // const viewportPoint = view.viewport.pointFromPixel(even.position);
                // scan.viewport.panTo(viewportPoint, true);
                // console.log(evnt,'----------')
            }
            const canvasClick = (even)=>{
                if(even.quick){
                    let zoom = view.viewport.getZoom()
                    if(even.shift)  newZoon = 40;
                    const viewportPoint = view.viewport.pointFromPixel(even.position);
                    scan.viewport.zoomTo(zoom);
                    scan.viewport.panTo(viewportPoint, true);
                }
            }

            const initView= ()=>{
                view = new OpenSeadragon({
                    element: document.getElementById('domeTwoView'),
                    showZoomControl: false,
                    showHomeControl: false,
                    showFullPageControl: false,
                    // zoomPerClick: 0,
                    // visibilityRatio: 1,
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

                scan = new OpenSeadragon({
                    element: document.getElementById('domeTwoScan'),
                    showZoomControl: false,
                    showHomeControl: false,
                    showFullPageControl: false,
                    // zoomPerClick: 0,
                    // visibilityRatio: 1,
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


                const emitZoomScan = ()=>{
                    view.viewport.zoomTo(scan.viewport.getZoom());
                    view.viewport.panTo(scan.viewport.getCenter(), true);
                }
                const emitScanChange = (even)=>{
                    view.viewport.panTo(scan.viewport.getCenter(), true);
                    // const viewportPoint = view.viewport.pointFromPixel(even.position);
                    // scan.viewport.panTo(viewportPoint, true);
                    // console.log(evnt,'----------')
                }
                scan.addHandler('canvas-scroll', emitZoomScan);
                scan.addHandler('canvas-drag-end', emitScanChange);
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

<style scoped lang="scss">
    .domeTwo{
        height: 400px;
        .box{
            display: flex;
            flex: 1;
        }
        .view {
            flex: 1;
            height: 300px;
            background: rebeccapurple;
        }
        .scan {
            flex: 1;
            height: 300px;
            background: salmon;
        }
    }

</style>