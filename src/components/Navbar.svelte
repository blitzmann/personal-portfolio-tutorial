<script lang="ts">
    // This navbar is loosely based on the tutorial here: https://w3collective.com/responsive-navbar-tailwind-css/
    // The source code for the tutorial is located here: https://gist.github.com/w3collective/516123bb36c4b8333235d9eed88c1457
    // Most of the changes to this navbar have been done to "sveltify" it, and change some styling options.

    // Imports from Svelte's animation libraries for animating the mobile navbar
    import { fade, fly } from 'svelte/transition';
    import { quadInOut } from 'svelte/easing';
    // The Definition file for our Navbar Links
    import links from '../lib/navbar-links';
    // Font Awesome for Svelte
    import Fa from 'svelte-fa';
    import { faBars, faTimes } from '@fortawesome/free-solid-svg-icons';
    // Small utility for "click outside" functionality on the mobile navbar
    import { clickOutside } from '../lib/clickOutside';

    // Variable setup for whether or not the menu is open
    let menuIsOpen = false;
    // Variable setup for tracking the current width of the navbar element
    let navWidth;

    // $: Reactive Declaration, where if the navbarwidth changes, we close the nav drawer.
    $: if (navWidth) {
        menuIsOpen = false;
    }

    // Function that closes the nav drawer when the user clicks outside
    function handleClickOuside(event) {
        menuIsOpen = false;
    }
    // Function that toggles the menu open/closed
    function toggleMenu() {
        menuIsOpen = !menuIsOpen;
    }
</script>

<nav
    class="flex flex-wrap items-center justify-between md:py-5 md:px-10 lg:px-20 bg-white shadow-sm"
    bind:clientWidth={navWidth}
    use:clickOutside
    on:click_outside={handleClickOuside}
>
    <span class="font-semibold uppercase text-lg py-7 pl-10 md:py-0 md:pl-0">
        <a href="/">Ryan Holmes</a>
    </span>
    <div class="flex md:hidden py-7 pr-10">
        <button
            id="hamburger"
            class="active:border-none focus:outline-none"
            on:click|preventDefault={toggleMenu}
        >
            {#if !menuIsOpen}
                <Fa icon={faBars} />
            {:else}
                <Fa icon={faTimes} />
            {/if}
        </button>
    </div>
    <div class="toggle hidden md:w-auto md:flex text-right text-bold mt-5 mt-0 border-none">
        {#each links as link, idx}
            <a
                href={link.url}
                rel="external"
                class="block md:inline-block text-gray-600 hover:text-gray-900 px-10 md:px-5 py-3 border-b border-gray-100 last:border-none md:border-none"
            >
                {link.title}
            </a>
        {/each}
    </div>
    <!--Mobile Links-->
    {#if menuIsOpen}
        <div
            in:fly={{
                delay: 0,
                duration: 300,
                y: 0,
                x: navWidth * -1,
                opacity: 0.5,
                easing: quadInOut
            }}
            out:fly={{
                delay: 0,
                duration: 300,
                y: 0,
                x: navWidth * -1,
                opacity: 0.5,
                easing: quadInOut
            }}
            class={`w-full text-bold mt-0 pb-1 md:hidden absolute bg-white top-[83px] z-10`}
        >
            {#each links as link, idx}
                <a
                    href={link.url}
                    rel="external"
                    on:click={toggleMenu}
                    class="block md:inline-block text-gray-600 hover:text-gray-900 px-10 md:px-5 py-3 border-b border-gray-100 last:border-none md:border-none"
                >
                    <Fa icon={link.icon} fw class="inline mr-2" />
                    {link.title}
                </a>
            {/each}
        </div>
    {/if}
</nav>
