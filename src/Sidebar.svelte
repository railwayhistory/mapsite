<script>
    import {
        currLayer, numLayer, setMainLayer, setNumLayer
    } from './Map.svelte';
    import InfoPanel from './InfoPanel.svelte';
    import AboutPanel from './AboutPanel.svelte';

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

    function toggle_overview() {
        setMainLayer("overview")
    }

    function toggle_passenger() {
        setMainLayer("passenger")
    }

    function toggle_detail() {
        setMainLayer("detail")
    }

    function toggle_numbers() {
        setNumLayer(!numLayerValue);
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
    .icons .overview span {
        background-position-x: -300px;
        height: 30px !important;
        margin-bottom: 0 !important;
    }
    .icons li.overview {
        margin-bottom: 0 !important;
    }
    .icons .pax span {
        background-position-x: -350px;
        height: 28px !important;
        margin-top: 0 !important;
        margin-bottom: 0 !important;
    }
    .icons li.pax {
        margin-top: 0 !important;
        margin-bottom: 0 !important;
    }
    .icons .construction span {
        background-position-x: -400px;
        height: 30px !important;
        margin-top: 0 !important;
    }
    .icons li.construction {
        margin-top: 0 !important;
    }

    .icons .numbers span {
        background-position-x: -450px;
        height: 32px !important;
        margin-top: 0 !important;
    }

    .icons .info span {
        background-position-x: 0;
        margin-bottom: 0 !important;
    }

    .icons .info {
        padding-top: 4px;
    }

    .icons .about span {
        background-position-x: -250px;
    }

    .icons .about {
    }

    .content {
        position: relative;
        padding: 2px 0;
        margin: 0;
    }

    @media (min-width: 550px) {
        .content {
            width: 300px;
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
        <li class="overview">
            <span class="{ currLayerValue === "overview" ? 'active' : ''}" on:click={toggle_overview} title ="Switch to the overview layer">
                i
            </span>
        </li>
        <li class="pax">
            <span class="{ currLayerValue === "passenger" ? 'active' : ''}" on:click={toggle_passenger} title ="Switch to the passenger service layer">
                i
            </span>
        </li>
        <li class="construction">
            <span class="{ currLayerValue === "detail" ? 'active' : ''}" on:click={toggle_detail} title="Switch to the (incomplete) detail layer">
                i
            </span>
        </li>

        <li class="numbers">
            <span class="{ numLayerValue  ? 'active' : ''}" on:click={toggle_numbers} title="{ numLayerValue ? 'Disable' : 'Show' } line numbers">
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
        </div>
    {/if }
</div>

