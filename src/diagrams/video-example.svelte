<script>
    import { onMount } from 'svelte';
    import IntersectionObserver from './intersection-observer.svelte';

    export let example;

    let playButton;
    let overlay;
    let video;
    let shortVideo = true;

    let isMobile = false;
    let mounted = false;

    onMount(() => {
        if (document.documentElement.clientWidth <= 720) {
            isMobile = true;
        }
        mounted = true;
    })

    function play() {
        if (shortVideo === true) {
            // playButton.classList.add("hideVideoOverlay")
            // overlay.classList.add("hideVideoOverlay")
            let playPromise = video.play()
            if (playPromise !== undefined) {
                playPromise.then(_ => {
                // Automatic playback started!
                // Show playing UI.
                })
                .catch(error => {
                // Auto-play was prevented
                // Show paused UI.
                });
            }
        }
    }

    function pause() {
        if (shortVideo === true) {
            // playButton.classList.remove("hideVideoOverlay")
            // overlay.classList.remove("hideVideoOverlay")
            video.pause()
        }
    }

    function showShortVideo() {
        shortVideo = true;
        pause();
    }

    function hideShortVideoOverlay() {
        if (shortVideo === false) {
            // playButton.classList.add("hideVideoOverlay")
            // overlay.classList.add("hideVideoOverlay")
        }
    }

    function showLongVideo() {
        shortVideo = false;
        // hideShortVideoOverlay();
    }

</script>

<style>

    figure {
        padding-bottom: 0em;
    }

    video {
        width: 100%;
        display: block;
        cursor: pointer;
        border: 1px solid var(--gray-border);
    }

    /* .overlay {
        height: 100%;
        width: 100%;
        position: absolute;
        top: 0px;
        left: 0px;
        z-index: 2;
        background: black;
        opacity: 0.2;
    } */

    .video-wrap {
      position: relative;
      margin-bottom: 0.5rem;
      /* border: 1px solid var(--gray-border); */
      min-height: 255px;
    }

    /* .video-wrap .play-button {
      position: absolute;
      width: 5rem;
      height: 5rem;
      left: 50%;
      top: 50%;
      transform: translate(-50%, -50%);
      z-index: 3;
    } */

    /* .video-wrap .play-button svg {
      fill: #ffffff;
    } */

    /* adjust your icon size, add different breakpoints as needed */
    /* @media screen and (max-width: 768px) {
      .video-wrap .play-button {
        width: 4rem;
        height: 4rem;
      }
    } */

    #video-lengths {
        margin-top: 0.35rem;
    }

    #video-lengths > span {
        display: inline;
    }

    .video-selected {
        font-weight: bold;
        /* border-bottom: 1px solid var(--blue); */
    }

    button {
        padding: 0;
        border: none;
        outline: none;
        background: none;
        color: var(--blue);
        text-transform: uppercase;
        font-size: 0.85em;
        cursor: pointer;
    }

    /* .example-title {
		margin-bottom: 0.5em;
	} */

	/* .example > a {
		border-bottom: none;
    } */

    @media (max-width: 1178px) {
        .video-wrap {
            min-height: 290px;
        }

        figure {
            padding-bottom: 1em;
        }
    }

    @media(max-width: 1000px) {
        .video-wrap {
            min-height: 268px;
        }
    }

    @media(max-width: 768px) {
        .video-wrap {
            min-height: calc((100vw - 16px) * 9 / 16 + 2px);
        }
    }


    :global(.hideVideoOverlay) {
        display: none;
    }

</style>

<figure id={example.bibtex}>

    <div class="video-wrap">

        <!-- <div class="play-button" bind:this={playButton}>
            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 311.69 311.69"><path d="M155.84,0A155.85,155.85,0,1,0,311.69,155.84,155.84,155.84,0,0,0,155.84,0Zm0,296.42A140.58,140.58,0,1,1,296.42,155.84,140.58,140.58,0,0,1,155.84,296.42Z"></path><polygon points="218.79 155.84 119.22 94.34 119.22 217.34 218.79 155.84"></polygon></svg>
        </div>

        <div class="overlay" bind:this={overlay}></div> -->

        <IntersectionObserver once={true} let:intersecting={intersecting}>
            {#if intersecting}
                {#if mounted && (!isMobile)}
                    {#if shortVideo}
                        <video bind:this={video} autoplay controls playsinline muted loop>
                            <source src={example.teaser} type="video/mp4">
                            Your browser does not support HTML5 video.
                        </video>
                    {:else}
                        <video bind:this={video} autoplay controls playsinline muted loop>
                            <source src={example.video} type="video/mp4">
                            Your browser does not support HTML5 video.
                        </video>
                    {/if}

                {:else if mounted && (isMobile)}
                    {#if shortVideo}
                        <video bind:this={video} autoplay={!isMobile} controls playsinline muted poster={example.poster} preload="meta">
                            <source src={example.teaser} type="video/mp4">
                            Your browser does not support HTML5 video.
                        </video>
                    {:else}
                        <video bind:this={video} autoplay={!isMobile} controls playsinline muted poster={example.poster} preload="meta">
                            <source src={example.video} type="video/mp4">
                            Your browser does not support HTML5 video.
                        </video>
                    {/if}

                {/if}
            {/if}
        </IntersectionObserver>

    </div>

    <!-- <div class="example-title"><a href={example.url}>  {example.title}</a><d-cite key={example.bibtex}></d-cite></div> -->

    <figcaption>
        <a class="video-number" href="#{example.bibtex}">{example.id}</a>: In "<a href={example.url}>{example.title}</a>" <d-cite key={example.bibtex}></d-cite>, {example.caption}
            <div id="video-lengths">
                <span>
                    Playing
                    <button class={shortVideo === true ? "video-selected" : ""} on:click={() => { pause(); shortVideo = true }}>Preview</button>,
                    click to play
                    <button class={shortVideo === false ? "video-selected" : ""} on:click={() => { pause(); shortVideo = false }}>Full Video</button>.
                </span>
            </div>
    </figcaption>

</figure>
