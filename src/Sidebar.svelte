<script>
    import { setMainLayer } from './Map.svelte';
    import InfoPanel from './InfoPanel.svelte';
    import LayersPanel from './LayersPanel.svelte';

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
          top: 0;
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
    .icons .info span {
        background-position-x: 0;
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
        <li class="layers">
            <span class="{ sidebar === "layers" ? 'active' : ''}" on:click={toggle_layers}>l</span>
        </li>
        <li class="info">
            <span class="{ sidebar === "info" ? 'active' : ''}" on:click={toggle_info}>
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
            {#if sidebar === "layers" }
                <LayersPanel />
            {/if }
        </div>
    {/if }
</div>

