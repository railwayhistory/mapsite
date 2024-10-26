<script context="module">

    import {writable} from 'svelte/store';

    let mainLayers;
    let mapLayers;
    let map;
    var localCurrLayer = 'overview';
    var localNumLayer = true;
    var localLatinLabels = false;
    export const currLayer = writable("overview");
    export const numLayer = writable(true);
    export const latinLabels = writable(false);
    export const currDetail = writable(0);

    export function setMainLayer(layer) {
        currLayer.set(layer);
        localCurrLayer = layer;
        setLayer()
    }

    export function setNumLayer(layer) {
        numLayer.set(layer);
        localNumLayer = layer;
        setLayer()
    }

    export function setLatinLabels(enable) {
        latinLabels.set(enable);
        localLatinLabels = enable;
        setLayer()
    }

    function setLayer() {
        /*
        let layers = new Array();
        if (localLatinLabels) {
            layers.push(mainLayers[localCurrLayer]['latinLayer']);
        }
        else {
            layers.push(mainLayers[localCurrLayer]['layer']);
        }
        if (localNumLayer) {
            layers.push(mainLayers[localCurrLayer]['numLayer']);
        }
        */
        mapLayers[1] = railwayLayer(
            localCurrLayer, localNumLayer, localLatinLabels
        );
        map.setLayers(mapLayers);
        map.getView().setMaxZoom(mainLayers[localCurrLayer]['maxZoom']);
        updateHistory(map.getView());
    }

    function updateHistory(view) {
        let zoom = Math.round(view.getZoom());
        var center = transform(
          view.getCenter(), "EPSG:3857", "EPSG:4326"
        );
        var x = (Math.round(center[1] * 10000) / 10000);
        var y = (Math.round(center[0] * 10000) / 10000);

        currDetail.set(mainLayers[localCurrLayer].detail[zoom]);
        let initView = localCurrLayer;
        if (!localNumLayer) {
            initView = initView + "-nonum";
        }
        if (localLatinLabels) {
            initView = initView + "-latin";
        }
        initView = initView + "@" + zoom + "/" + x + "/" + y;

        window.history.replaceState(
            window.history.state, document.title,
            "#" + initView
        );
        localStorage.setItem("initView", initView);
    }

    function clickLocation() {
        if ('geolocation' in navigator) {
            navigator.geolocation.getCurrentPosition((position) => {
                var center = transform(
                    [position.coords.longitude, position.coords.latitude],
                    "EPSG:4326", "EPSG:3857",
                );
                map.getView().setCenter(center);
            })
        }
    }

    function getInitView() {
        let initView = {
            layer: "overview",
            numLayer: true,
            center: [9.538, 54.0728], zoom: 9
        };

        let parseLocation = function(value) {
            if (value === null || value.length === 0) {
                return
            }
            var parts = value.split("@");
            if (parts.length === 2) {
                let layer = parts[0];
                let numLayer = true;
                let latinLabels = false;
                if (layer.endsWith("-latin")) {
                    layer = layer.slice(-6);
                    latinLabels = true;
                }
                if (layer.endsWith("-nonum")) {
                    layer = layer.slice(-6);
                    numLayer = false;
                }
                if (!(layer in mainLayers)) {
                    layer = "overview";
                }
                var mapparts = parts[1].split("/");
                if (mapparts.length === 3) {
                    initView = {
                        layer: layer,
                        numLayer: numLayer,
                        latinLabels: latinLabels,
                        zoom: parseInt(mapparts[0], 10),
                        center: [
                            parseFloat(mapparts[2]),
                            parseFloat(mapparts[1]),
                        ]
                    };
                }
            }
        }
        parseLocation(localStorage.getItem("initView"));
        parseLocation(window.location.hash.slice(1));
        return initView;
    }

    function railwayLayer(curr, num, latin) {
        let layers = new Array();
        if (latin) {
            layers.push(mainLayers[curr]['latinLayer']);
        }
        else {
            layers.push(mainLayers[curr]['layer']);
        }
        if (num) {
            layers.push(mainLayers[curr]['numLayer']);
        }
        return new Group({ layers: layers });
    }

</script>
<script>
    import { onMount } from 'svelte';
    import { Map, View } from 'ol';
    import {
        Attribution, ScaleLine, defaults as defaultControls
    } from 'ol/control';
    import { Group, Tile } from 'ol/layer';
    import { transform } from 'ol/proj';
    import { XYZ } from 'ol/source';
    import PageFoot from './PageFoot.svelte';

    let component;

    onMount(() => {
        const rwhBase = "https://map.railwayhistory.org/rail/";
        mainLayers = {
            'overview': {
                'layer': new Tile({
                    minZoom: 4,
                    source: new XYZ({
                        url: rwhBase + "el/{z}/{x}/{y}.png",
                        opaque: false,
                        tilePixelRatio: 2,
                    }),
                }),
                'latinLayer': new Tile({
                    minZoom: 4,
                    source: new XYZ({
                        url: rwhBase + "el-lat/{z}/{x}/{y}.png",
                        opaque: false,
                        tilePixelRatio: 2,
                    }),
                }),
                'numLayer': new Tile({
                    minZoom: 4,
                    source: new XYZ({
                        url: rwhBase + "el-num/{z}/{x}/{y}.png",
                        opaque: false,
                        tilePixelRatio: 2,
                    }),
                }),
                'detail': [
                    0, 0, 0, 0, 0,
                    0, 0, 1, 1, 2,
                    2,
                ],
                'maxZoom': 10,
            },
            'passenger': {
                'layer': new Tile({
                    minZoom: 4,
                    source: new XYZ({
                        url: rwhBase + "pax/{z}/{x}/{y}.png",
                        opaque: false,
                        tilePixelRatio: 2,
                    }),
                }),
                'latinLayer': new Tile({
                    minZoom: 4,
                    source: new XYZ({
                        url: rwhBase + "pax-lat/{z}/{x}/{y}.png",
                        opaque: false,
                        tilePixelRatio: 2,
                    }),
                }),
                'numLayer': new Tile({
                    minZoom: 4,
                    source: new XYZ({
                        url: rwhBase + "pax-num/{z}/{x}/{y}.png",
                        opaque: false,
                        tilePixelRatio: 2,
                    }),
                }),
                'detail': [
                    0, 0, 0, 0, 0,
                    0, 0, 1, 1, 2,
                    2,
                ],
                'maxZoom': 10,
            },
            'detail': {
                'layer': new Tile({
                    minZoom: 4,
                    source: new XYZ({
                        url: rwhBase + "el/{z}/{x}/{y}.png",
                        opaque: false,
                        tilePixelRatio: 2,
                    }),
                }),
                'latinLayer': new Tile({
                    minZoom: 4,
                    source: new XYZ({
                        url: rwhBase + "el-lat/{z}/{x}/{y}.png",
                        opaque: false,
                        tilePixelRatio: 2,
                    }),
                }),
                'numLayer': new Tile({
                    minZoom: 4,
                    source: new XYZ({
                        url: rwhBase + "el-num/{z}/{x}/{y}.png",
                        opaque: false,
                        tilePixelRatio: 2,
                    }),
                }),
                'detail': [
                    0, 0, 0, 0, 0,
                    0, 0, 1, 1, 2,
                    3, 3, 4, 4, 5,
                    5, 5, 5, 5, 5,
                ],
                'maxZoom': 15,
            }
        };


        var initView = getInitView();

        mapLayers = [
            new Group({
                layers: [
                    new Tile({
                        source: new XYZ({
                            url: "https://map.railwayhistory.org/base/{z}/{x}/{y}.png",
                            tilePixelRatio: 2,
                        })
                    }),
                    new Tile({
                        source: new XYZ({
                            url: "https://hillshading.waymarkedtrails.org/srtm/{z}/{x}/{-y}.png",
                            tilePixelRation: 2,
                        }),
                        opacity: 0.13,
                    }),
                    new Tile({
                        source: new XYZ({
                            url: rwhBase + "border/{z}/{x}/{y}.png",
                            tilePixelRatio: 2,
                            opaque: false,
                        })
                    }),
                ]
            }),
            railwayLayer(
                initView.layer, initView.numLayer, initView.latinLabels
            ),
        ];
        currLayer.set(initView.layer);
        localCurrLayer = initView.layer;
        numLayer.set(initView.numLayer);
        localNumLayer = initView.numLayer;
        latinLabels.set(initView.latinLabels);
        localLatinLabels = initView.latinLabels;
        map = new Map({
            target: component,
            view: new View({
                center: transform(initView.center, "EPSG:4326", "EPSG:3857"),
                zoom: initView.zoom,
                maxZoom: mainLayers[initView.layer]['maxZoom'],
                constrainResolution: true
            }),
            controls: defaultControls({
                attribution: false,
                rotate: false,
            }).extend([new ScaleLine()]),
            layers: mapLayers,
        });

        map.on('moveend', mapMoveEnd);
        updateHistory(map.getView());
    });

    function resizeMap() {
        setTimeout( function() { map.updateSize();}, 200);
    }

    function mapMoveEnd(evt) {
        updateHistory(evt.map.getView())
    }
</script>

<style>
    div {
        position: relative;
        flex-grow: 1;
    }

    .controls {
        position: absolute;
        top: 74px;
        right: 7px;
        z-index: 999;
    }

    .controls button {
        display: block;
        border: none;
        padding: 0;
        height: 32px;
        width: 32px;
        background-color: rgba(0,0,0,0);
        background-image: url("../img/icons.svg");
        background-position-y: 0px;
        color: rgb(0,0,0,0);
        margin-bottom: 3px;
    }
    .controls button:hover {
        background-position-y: -50px
    }
    .location {
        background-position-x: -200px;
    }
</style>

<svelte:window on:resize={resizeMap}/>

<div bind:this={component} class="map">
    <div class="controls">
         <button class="location" type="button" title="Go to my location" on:click={clickLocation}>?</button>
    </div>
    <PageFoot></PageFoot>
</div>
