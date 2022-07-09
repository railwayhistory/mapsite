<script context="module">

    import {writable} from 'svelte/store';

    let mainLayers;
    let mapLayers;
    let map;
    var localCurrLayer = 'overview';
    export const currLayer = writable("overview");
    export const currDetail = writable(0);

    export function setMainLayer(layer) {
        localCurrLayer = layer;
        currLayer.set(layer);
        mapLayers[2] = mainLayers[layer]['layer'];
        map.setLayers(mapLayers);
        map.getView().setMaxZoom(mainLayers[layer]['maxZoom']);
        updateHistory(map.getView());
    }

    export function getMainLayer(layer) {
        currLayer
    }

    function updateHistory(view) {
        let zoom = Math.round(view.getZoom());
        var center = transform(
          view.getCenter(), "EPSG:3857", "EPSG:4326"
        );
        var x = (Math.round(center[1] * 10000) / 10000);
        var y = (Math.round(center[0] * 10000) / 10000);

        currDetail.set(mainLayers[localCurrLayer].detail[zoom]);

        window.history.replaceState(
            window.history.state, document.title,
            "#" + localCurrLayer + "@" + zoom + "/" + x + "/" + y
        );
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

</script>
<script>
    import { onMount } from 'svelte';
    import { Map, View } from 'ol';
    import {
        Attribution, ScaleLine, defaults as defaultControls
    } from 'ol/control';
    import { Tile } from 'ol/layer';
    import { transform } from 'ol/proj';
    import { XYZ } from 'ol/source';
    import PageFoot from './PageFoot.svelte';

    let component;

    onMount(() => {
        mainLayers = {
            'overview': {
                'layer': new Tile({
                    minZoom: 4,
                    source: new XYZ({
                        url: "https://map.railwayhistory.org/rail/se/{z}/{x}/{y}.png",
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
                        url: "https://map.railwayhistory.org/rail/sp/{z}/{x}/{y}.png",
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
                        url: "https://map.railwayhistory.org/rail/le/{z}/{x}/{y}.png",
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
                'maxZoom': 13,
            }
        };


        var init_view = {
            layer: "overview", center: [9.538, 54.0728], zoom: 9
        };
        var hashparts = window.location.hash.slice(1).split("@");
        if (hashparts.length === 2) {
            if (!(hashparts[0] in mainLayers)) {
                hashparts[0] = "overview";
            }
            var mapparts = hashparts[1].split("/");
            if (mapparts.length === 3) {
                init_view = {
                    layer: hashparts[0],
                    zoom: parseInt(mapparts[0], 10),
                    center: [
                        parseFloat(mapparts[2]),
                        parseFloat(mapparts[1]),
                    ]
                };
            }
        }

        mapLayers = [
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
            mainLayers[init_view.layer]['layer'],
        ];
        currLayer.set(init_view.layer);
        localCurrLayer = init_view.layer;
        map = new Map({
            target: component,
            view: new View({
                center: transform(init_view.center, "EPSG:4326", "EPSG:3857"),
                zoom: init_view.zoom,
                maxZoom: mainLayers[init_view.layer]['maxZoom'],
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
        top: 147px;
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
