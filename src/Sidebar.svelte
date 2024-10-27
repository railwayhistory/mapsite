<script>
    import {
        currLayer, numLayer, setMainLayer, setNumLayer
    } from './Map.svelte';
    import InfoPanel from './InfoPanel.svelte';
    import AboutPanel from './AboutPanel.svelte';
    import LayersPanel from './LayersPanel.svelte';

    let currLayerValue;
    const unsubscribe = currLayer.subscribe(value => {
        currLayerValue = value;
    });

    let numLayerValue;
    const unsubscribeNumLayer = numLayer.subscribe(value => {
        numLayerValue = value;
    });

    let sidebar = null;

    function toggle_info() {
        if (sidebar === "info") {
            sidebar = null
        }
        else {
            sidebar = "info"
        }
    }

    function toggle_layers() {
        if (sidebar === "layers") {
            sidebar = null
        }
        else {
            sidebar = "layers"
        }
    }

    function toggle_about() {
        if (sidebar === "about") {
            sidebar = null
        }
        else {
            sidebar = "about"
        }
    }

    function close_sidebar() {
        sidebar = null
    }
</script>

<style>
    .sidebar {
          position: relative;
          background-color: #fcfbf8;
    }
    .sidebar > div {
        border-left: 1px solid #576793;
        overflow-y: scroll;
    }
    .icons {
          list-style: none;
          position: absolute;
          top: 112px;
          left: -40px;
          margin: 0;
          margin-top: 3px;
          padding: 0;
          width: 40px;
    }
    .icons li {
        padding: 0px 1px;
        margin: 0;
        margin-bottom: 3px;
    }
    .icons span {
        display: block;
        width: 32px;
        height: 32px;
        padding: 0;
        background-repeat: no-repeat;
        background-image: url("../img/icons.svg");
        color: rgb(0,0,0,0);
        background-position-y: 0px;
    }
    .icons span:hover {
        background-position-y: -50px;
    }
    .icons span.active {
        background-position-y: -100px;
    }
    .icons span.active:hover {
        background-position-y: -150px;
    }

    .icons .layers span {
        background-position-x: -50px;
        height: 32px !important;
        margin-top: 0 !important;
    }

    .icons .info span {
        background-position-x: 0;
        margin-bottom: 0 !important;
    }

    .icons .about span {
        background-position-x: -250px;
    }

    .icons .about {
    }

    .content {
        position: relative;
        padding:  0;
        margin: 0;
    }

    @media (min-width: 550px) {
        .content {
            width: 300px;
            height: 100%;
        }
    }

    @media (max-width: 550px) {
        .content {
            position: fixed;
            left: 0;
            top: 0;
            right: 0;
            bottom: 0;
            z-index: 999;
            background-color: white;
        }
    }

    .close {
        display: block;
        position: absolute;
        right: 0;
        top: 0;
        margin: 2px;
        border: 2px solid #202e53;
        border-radius: 4px;
        width: 16px;
        height: 16px;
        text-align: center;
        font-size: 15px;
        font-weight: bold;
        color: #202e53;
        cursor: pointer;
    }
    .close:hover {
        background-color: #c4b06d;
    }
</style>

<div class="sidebar">
    <ul class="icons">
        <li class="layers">
            <span class="{ sidebar === "layers" ? 'active' : ''}" on:click={toggle_layers} title="Configure map layers">
                i
            </span>
        </li>
        <li class="info">
            <span class="{ sidebar === "info" ? 'active' : ''}" on:click={toggle_info} title="Show information and map key">
                i
            </span>
        </li>
        <li class="about">
            <span class="{ sidebar === "about" ? 'active' : ''}" on:click={toggle_about} title="About the map">
                i
            </span>
        </li>
    </ul>

    {#if !(sidebar === null) }
        <div class="content">
            <span class="close" href="#" on:click={close_sidebar}>&times;</span>
            {#if sidebar === "info" }
                <InfoPanel />
            {/if }
            {#if sidebar === "about" }
                <AboutPanel />
            {/if }
            {#if sidebar === "layers" }
                <LayersPanel />
            {/if }
        </div>
    {/if }
</div>

