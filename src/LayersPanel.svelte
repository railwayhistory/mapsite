<script>
    import { onDestroy } from 'svelte';
    import {
        currLayer, latinLabels, numLayer, setMainLayer, setLatinLabels,
        setNumLayer
    } from "./Map.svelte";

    let currLayerValue;
    const unsubscribe = currLayer.subscribe(value => {
        currLayerValue = value;
    });

    let numLayerValue;
    const unsubscribeNumLayer = numLayer.subscribe(value => {
        numLayerValue = value;
    });

    let latinLabelsValue;
    const unsubscribeLatinLabels = latinLabels.subscribe(value => {
        latinLabelsValue = value;
    });

    function selectOverview() {
        setMainLayer("overview");
    }

    function selectPassenger() {
        setMainLayer("passenger");
    }

    function selectDetail() {
        setMainLayer("detail");
    }

    function enableNumbers() {
        setNumLayer(true);
    }
    function disableNumbers() {
        setNumLayer(false);
    }

    function enableLatin() { setLatinLabels(true); }
    function disableLatin() { setLatinLabels(false); }

    onDestroy(unsubscribe);
</script>

<style>
    .layers {
        padding: 0px;
        margin: 0px;
    }

    h1, h2 {
        font-size: 1.2rem;
        font-weight: bold;
        padding: 2px 8px 10px;
        margin: 0;
    }
    ul {
        list-style: none;
        padding: 0 0.2rem;
        margin: 0;
        margin-bottom: 1.5rem;
    }
    .select {
        cursor: pointer;
        margin: 0;
        border-color: #576793;
        border-width: 1px;
        border-style: solid solid none solid;
    }
    .select:first-child {
        border-radius: 3px 3px 0 0;
    }
    .select:last-child {
        border-radius: 0 0 3px 3px;
        border-bottom-style: solid;
    }
    .select:hover {
        background-color: #e3d9bd;
    }
    .active {
        background-color: #576793;
        color: #ffffff;
    }
    .active:hover {
        background-color: #576793;
    }
    .select h3 {
        padding: 4px 8px 2px;
        font-size: 1rem;
        font-weight: normal;
        margin: 0;
    }
    .select p {
        padding: 4px 8px 4px;
        font-size: 0.8rem;
        margin: 0;
    }
</style>

<div class="layers">
    <h1>Layers</h1>
    <ul>
        <li class="select{ currLayerValue === 'overview' ? ' active' : '' }" on:click={selectOverview}>
                <h3>Overview</h3>
                <p>
                    Small scale overview map with colorized electrification.
                </p>
        </li>
        <li class="select{ currLayerValue === 'passenger' ? ' active' : '' }" on:click={selectPassenger}>
                <h3>Passenger services</h3>
                <p>
                    Small scale overview map only showing lines with
                    passenger services.
                </p>
        </li>
        <li class="select{ currLayerValue === 'detail' ? ' active' : '' }" on:click={selectDetail}>
                <h3>Details <i>(Under Construction)</i></h3>
                <p>
                    Large scale map showing details. Under construction and
                    severely incomplete.
                </p>
        </li>
    </ul>
    <h2>Line Numbers</h2>
    <ul>
        <li class="select{ numLayerValue  ? ' active' : ''}" on:click={enableNumbers}>
            <h3>Show line numbers</h3>
            <p>
                Line numbers are either official or internal to the project.
            </p>
        </li>
        <li class="select{ numLayerValue  ? '' : ' active'}" on:click={disableNumbers}>
            <h3>Disable line number</h3>
            <p>
                Hide line numbers for less clutter.
            </p>
        </li>
    </ul>
    <h2>Label Script</h2>
    <ul>
        <li class="select{ latinLabelsValue ? '' : ' active'}" on:click={disableLatin}>
            <h3>Local script</h3>
            <p>
                Show labels in local script, e.g., Cyrillic, Greek, etc.
            </p>
        </li>
        <li class="select{ latinLabelsValue ? ' active' : ''}" on:click={enableLatin}>
            <h3>Latin script</h3>
            <p>
                Transliterate labels into Latin script.
            </p>
        </li>
    </ul>

</div>
