<script>
    import { onDestroy } from 'svelte';
    import { currLayer, setMainLayer } from "./Map.svelte";

    let currLayerValue;
    const unsubscribe = currLayer.subscribe(value => {
        currLayerValue = value;
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

    onDestroy(unsubscribe);
</script>

<style>
    .layers {
        padding: 0px;
        margin: 0px;
    }

    h1 {
        font-size: 1.2rem;
        font-weight: bold;
        padding: 2px 8px 10px;
        margin: 0;
    }
    ul {
        list-style: none;
        padding: 0;
        margin: 0;
    }
    .select {
        padding: 1px 8px 4px;
        cursor: pointer;
        border-radius: 3px;
        margin: 2px 0;
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
    .select h2 {
        font-size: 1rem;
        font-weight: normal;
        margin: 0;
        padding: 4px 0;
    }
    .select p {
        font-size: 0.8rem;
        margin: 0;
    }
</style>

<div class="layers">
    <h1>Layers</h1>
    <ul>
        <li class="select{ currLayerValue === 'overview' ? ' active' : '' }" on:click={selectOverview}>
                <h2>Overview</h2>
                <p>
                    Small scale overview map with colorized electrification.
                </p>
        </li>
        <li class="select{ currLayerValue === 'passenger' ? ' active' : '' }" on:click={selectPassenger}>
                <h2>Passenger services</h2>
                <p>
                    Small scale overview map only showing lines with
                    passenger services.
                </p>
        </li>
        <li class="select{ currLayerValue === 'detail' ? ' active' : '' }" on:click={selectDetail}>
                <h2>Details <i>(Under Construction)</i></h2>
                <p>
                    Large scale map showing details. Under construction and
                    severely incomplete.
                </p>
        </li>
    </ul>
</div>
