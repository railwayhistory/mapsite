<script>
    import { onDestroy } from 'svelte';
    import { currLayer, currDetail } from "./Map.svelte";

    let currLayerValue;
    const unsubscribe1 = currLayer.subscribe(value => {
        currLayerValue = value;
    });

    let currDetailValue;
    const unsubscribe2 = currDetail.subscribe(value => {
        currDetailValue = value;
    });


    onDestroy(() => {
        unsubscribe1();
        unsubscribe2();
    })
</script>

<style>
    .infoPanel {
        padding: 0px;
        margin: 0px;
    }

    h1 {
        font-size: 1.2rem;
        font-weight: bold;
        padding: 2px 8px 2px;
        margin: 0;
    }

    h2 {
        font-size: 1rem;
        font-weight: normal;
        margin: 0;
        padding: 12px 0px 4px 10px;
    }

    .mapKey {
        border-collapse: collapse;
    }
    .symbol {
        text-align: center;
        vertical-align: middle;
        background-color: white;
        padding: 0px 6px;
    }
    .track {
        display: inline-block;
        width: 3em;
        height: 0.4ex;
        border-top-style: solid;
        border-width: 2.5pt;
    }
    .empty {
        display: inline-block;
    }
    .desc {
        font-size: 0.8rem;
        padding-left: 10px;
    }
    .headdesc {
        font-size: 0.8rem;
        text-align: center;
    }

    .sorry {
        padding-left: 10px;
        font-size: 0.8rem;
    }
</style>


<div class="infoPanel">
    <h1>Map Key</h1>

    {#if currDetailValue <= 1 }
        {#if currLayerValue === 'passenger' }
            <table class="mapKey">
                <tr>
                    <td colspan="3">
                        <h2>Railway or Tram Line</h2>
                    </td>
                </tr>
                <tr>
                    <td colspan="2" class="symbol">
                        <span class="track"
                              style="border-color: #191919"
                        ></span>
                    </td>
                    <td class="desc">scheduled passenger service</td>
                </tr>
                <tr>
                    <td colspan="2" class="symbol">
                        <span class="track"
                              style="border-color: #4c4c4c"
                        ></span>
                    </td>
                    <td class="desc">heritage or tourist service</td>
                </tr>
                <tr>
                    <td colspan="2" class="symbol">
                        <span class="track"
                              style="border-color: #b2b2b2"
                        ></span>
                    </td>
                    <td class="desc">no passenger service</td>
                </tr>
                <tr>
                    <td colspan="2" class="symbol">
                        <span class="track"
                              style="border-color: #e5e5e5"
                        ></span>
                    </td>
                    <td class="desc">permanently closed line</td>
                </tr>
            </table>
        {:else}
            <table class="mapKey">
                <tr>
                    <td colspan="3">
                        <h2>Railway Line</h2>
                    </td>
                </tr>
                <tr>
                    <td class="headdesc">with</td>
                    <td class="headdesc">without</td>
                    <td></td>
                </tr>
                <tr>
                    <td colspan="2" class="headdesc">passenger<br>service</td>
                    <td></td>
                </tr>
                <tr>
                    <td class="symbol">
                        <span class="track"
                              style="border-color: #6c2b86"
                        ></span>
                    </td>
                    <td class="symbol">
                        <span class="track"
                              style="border-color: #6c2b86; opacity: 0.7"
                        ></span>
                    </td>
                    <td class="desc">
                        electrified with OLE ≥ 25 kV AC
                    </td>
                </tr>
                <tr>
                    <td class="symbol">
                        <span class="track"
                              style="border-color: #812c5c"
                        ></span>
                    </td>
                    <td class="symbol">
                        <span class="track"
                              style="border-color: #812c5c; opacity: 0.7"
                        ></span>
                    </td>
                    <td class="desc">
                        … &lt; 25 kV AC
                    </td>
                </tr>
                <tr>
                    <td class="symbol">
                        <span class="track"
                              style="border-color: #9b2321"
                        ></span>
                    </td>
                    <td class="symbol">
                        <span class="track"
                              style="border-color: #9b2321; opacity: 0.7"
                        ></span>
                    </td>
                    <td class="desc">
                        … ≥ 2000 V DC
                    </td>
                </tr>
                <tr>
                    <td class="symbol">
                        <span class="track"
                              style="border-color: #b64f0d"
                        ></span>
                    </td>
                    <td class="symbol">
                        <span class="track"
                              style="border-color: #b64f0d; opacity: 0.7"
                        ></span>
                    </td>
                    <td class="desc">
                        … &lt; 2000 V DC
                    </td>
                </tr>
                <tr>
                    <td class="symbol">
                        <span class="track"
                              style="border-color: #007e40"
                        ></span>
                    </td>
                    <td class="symbol">
                        <span class="track"
                              style="border-color: #007e40; opacity: 0.7"
                        ></span>
                    </td>
                    <td class="desc">
                        electrified with third rail
                    </td>
                </tr>
                <tr>
                    <td class="symbol">
                        <span class="track"
                              style="border-color: #53633b"
                        ></span>
                    </td>
                    <td class="symbol">
                        <span class="track"
                              style="border-color: #53633b; opacity: 0.7"
                        ></span>
                    </td>
                    <td class="desc">
                        electrified with fourth rail
                    </td>
                </tr>
                <tr>
                    <td class="symbol">
                        <span class="track"
                              style="border-color: #5a3a29"
                        ></span>
                    </td>
                    <td class="symbol">
                        <span class="track"
                              style="border-color: #5a3a29; opacity: 0.7"
                        ></span>
                    </td>
                    <td class="desc">
                        not electrified
                    </td>
                </tr>
                <tr>
                    <td class="symbol">
                        <span class="track"
                              style="border-color: #005387"
                        ></span>
                    </td>
                    <td></td>
                    <td class="desc">tram or light rail</td>
                </tr>
                <tr>
                    <td colspan="2" class="symbol">
                        <span class="track"
                              style="border-color: #8c8c8c"
                        ></span>
                    </td>
                    <td class="desc">closed permanently</td>
                </tr>
                <tr>
                    <td colspan="2" class="symbol">
                        <span class="track"
                              style="border-color: #a6a6a6"
                        ></span>
                    </td>
                    <td class="desc">tracks removed</td>
                </tr>
                <tr>
                    <td colspan="2" class="symbol">
                        <span class="track"
                              style="border-color: #e6e6e6"
                        ></span>
                    </td>
                    <td class="desc">closed before 1915</td>
                </tr>

            </table>
        {/if}
    {:else if currDetailValue == 2 }
        <table class="mapKey">
            <tr>
                <td colspan="3">
                    <h2>Line</h2>
                </td>
            </tr>
            <tr>
                <td colspan="2" class="symbol">
                    <span class="track"
                          style="border-color: #191919"
                    ></span>
                </td>
                <td class="desc">scheduled passenger service</td>
            </tr>
            <tr>
                <td colspan="2" class="symbol" style="border-color: #191919">
                    <span class="track" style="width: 0.4em"
                    ></span><span class="empty" style="width: 0.2em"
                    ></span><span class="track" style="width: 0.8em"
                    ></span><span class="empty" style="width: 0.2em"
                    ></span><span class="track" style="width: 0.8em"
                    ></span><span class="empty" style="width: 0.2em"
                    ></span><span class="track" style="width: 0.4em"
                    ></span>
                </td>
                <td class="desc">heritage or tourist service</td>
            </tr>
            <tr>
                <td colspan="2" class="symbol" style="border-color: #191919">
                    <span class="track" style="width: 0.175em"
                    ></span><span class="empty" style="width: 0.15em"
                    ></span><span class="track" style="width: 0.35em"
                    ></span><span class="empty" style="width: 0.15em"
                    ></span><span class="track" style="width: 0.35em"
                    ></span><span class="empty" style="width: 0.15em"
                    ></span><span class="track" style="width: 0.35em"
                    ></span><span class="empty" style="width: 0.15em"
                    ></span><span class="track" style="width: 0.35em"
                    ></span><span class="empty" style="width: 0.15em"
                    ></span><span class="track" style="width: 0.35em"
                    ></span><span class="empty" style="width: 0.15em"
                    ></span><span class="track" style="width: 0.175em"
                    ></span>
                </td>
                <td class="desc">no passenger service</td>
            </tr>
            <tr>
                <td colspan="2" class="symbol">
                    <span class="track"
                          style="border-color: #8c8c8c"
                    ></span>
                </td>
                <td class="desc">closed permanently</td>
            </tr>
            <tr>
                <td colspan="2" class="symbol">
                    <span class="track"
                          style="border-color: #a6a6a6"
                    ></span>
                </td>
                <td class="desc">tracks removed</td>
            </tr>
            <tr>
                <td colspan="2" class="symbol">
                    <span class="track"
                          style="border-color: #e6e6e6"
                    ></span>
                </td>
                <td class="desc">closed before 1915</td>
            </tr>
            <tr>
                <td colspan="3">
                    <h2>Electrification</h2>
                </td>
            </tr>
            <tr>
                <td colspan="2" class="symbol">
                    <span class="track"
                          style="border-color: #6c2b86"
                    ></span>
                </td>
                <td class="desc">
                    electrified with OLE ≥ 25 kV AC
                </td>
            </tr>
            <tr>
                <td colspan="2" class="symbol">
                    <span class="track"
                          style="border-color: #812c5c"
                    ></span>
                </td>
                <td class="desc">
                    … &lt; 25 kV AC
                </td>
            </tr>
            <tr>
                <td colspan="2" class="symbol">
                    <span class="track"
                          style="border-color: #9b2321"
                    ></span>
                </td>
                <td class="desc">
                    … ≥ 2000 V DC
                </td>
            </tr>
            <tr>
                <td colspan="2" class="symbol">
                    <span class="track"
                          style="border-color: #b64f0d"
                    ></span>
                </td>
                <td class="desc">
                    … &lt; 2000 V DC
                </td>
            </tr>
            <tr>
                <td colspan="2" class="symbol">
                    <span class="track"
                          style="border-color: #007e40"
                    ></span>
                </td>
                <td class="desc">
                    electrified with third rail
                </td>
            </tr>
            <tr>
                <td colspan="2" class="symbol">
                    <span class="track"
                          style="border-color: #53633b"
                    ></span>
                </td>
                <td class="desc">
                    electrified with fourth rail
                </td>
            </tr>
            <tr>
                <td colspan="3">
                    <h2>Tram or Light Rail</h2>
                </td>
            </tr>
            <tr>
                <td colspan="2" class="symbol">
                    <span class="track"
                          style="border-color: #005387"
                    ></span>
                </td>
                <td class="desc">tram or light rail</td>
            </tr>
            <tr>
                <td colspan="3">
                    <h2>Number of Tracks</h2>
                </td>
            </tr>
            <tr>
                <td colspan="2" class="symbol">
                    <span class="track"
                          style="border-width: 1.5pt"
                    ></span>
                </td>
                <td class="desc">single track</td>
            </tr>
            <tr>
                <td colspan="2" class="symbol">
                    <span class="track"
                          style="border-width: 2.5pt"
                    ></span>
                </td>
                <td class="desc">double track or more</td>
            </tr>
        </table>
    {:else if currLayerValue === 'detail'}
        <p class="sorry">Under Construction</p>
    {/if}
</div>

