<script>
    import "$lib/index.scss"
    import '$lib/fonts.scss';
    import banner from "$lib/img/banner.jpg"
    import { onMount } from 'svelte';

    let x = 0;
    let y = 0;
    let xp = 0;
    let yp = 0;
    let mouseEffectVar = false;
    let cursorStyle = "";

    const domousemove = (e) => {
        x = e.clientX
        y = e.clientY
    }

    onMount(async () => {
        setInterval( () => {
            xp += (x - xp) / 10;
            yp += (y - yp) / 10;
            cursorStyle = `left: ${xp}px; top: ${yp}px`;
        }, 12);
        
        await setInterval(typeJs, 120);
    });

    const textArray = [
        "Lethal company sync",
        "Open Source",
        "Sync your mods!",
    ];

    function Sleep(milliseconds) {
        return new Promise(resolve => setTimeout(resolve, milliseconds));
    }

    let heading = "";

    let stringIndex = 0; // Index of the current string in the array
    let charIndex = 0; // Index of the current character in the current string
    let isTyping = true; // Whether we are currently typing or erasing

    async function typeJs() {
        if (stringIndex < textArray.length) {
            const currentString = textArray[stringIndex];

            if (isTyping) {
                if (charIndex < currentString.length) {
                    heading += currentString.charAt(charIndex);
                    charIndex++;
                } else {
                    isTyping = false; 
                }
            } else {
                if (charIndex > 0) {
                    if(heading != "") {
                        cursorBlink = "cursor-blink"
                        await Sleep(3000)
                        cursorBlink = ""
                    }

                    heading = currentString.substring(0, charIndex - 1);
                    charIndex--;
                } else {
                    isTyping = true; // Switch back to typing mode
                    stringIndex++; // Move to the next string
                    
                    if (stringIndex >= textArray.length) {
                        stringIndex = 0; // Reset to the beginning of the array
                    }
                    
                    charIndex = 0; // Reset character index
                    heading = ""; // Clear the content for the new string
                }
            }
        }
    }

    const arrow = ">"
    let cursorBlink = "";
</script>

<svelte:window on:mousemove={domousemove} />

<section>
    <img class="banner" src={banner} alt="banner">
    <div class="cursor-element" style={cursorStyle}>
        <div class:mouseEffectVar class={`cursor-glow ${mouseEffectVar ? "active" : ""}`}></div>
        <span></span>
    </div>

    <h1 class="heading" >{arrow} {heading} <div class={`cursor ${cursorBlink}`}></div></h1>
    
    <p class="subtitle">Automatically setup and sync your mods with your friends</p>

    <div class="button-wrapper">
        <a on:mouseenter={() => mouseEffectVar = true}  on:mouseleave={() => mouseEffectVar = false} href="https://github.com/jonas-mtl/LCSync/releases/latest/download/LCSync.zip">
            Download now
        </a>
        <a on:mouseenter={() => mouseEffectVar = true}  on:mouseleave={() => mouseEffectVar = false} href="https://github.com/jonas-mtl/LCSync/" target="_blank">Browse Code</a>
    </div>

    <p class="credits">Made with ♡ by Trauubensaft </p>
</section>
