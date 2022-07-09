<script>
    import '../css/global.css';

    import PageHead from './PageHead.svelte';
    import Map from './Map.svelte';
    import Sidebar from './Sidebar.svelte';
</script>

<style>
    .screen {
        position: absolute;
        display: flex;
        flex-direction: row;
        height: 100%;
        width: 100%;
    }
</style>

<div class="screen">
    <PageHead></PageHead>
    <Map></Map>
    <Sidebar></Sidebar>
</div>
