<script lang="ts">
let { activeSection = "about" } = $props();

const patterns: Record<string, string[]> = {
    about: [
        "*",
        "•",
        "°",
        "+",
        "·",
        "✧",
        "✦",
        "◦",
        "▫",
        "▪",
        "•",
        "⚛",
        "⚬",
        "◌",
        "◍",
        "◎",
        "-",
        "^",
        "#",
        "@",
        "=",
        "<",
        ">",
    ],
    work: [
        "< />",
        "{ }",
        "[ ]",
        "||",
        "=>",
        "++",
        "--",
        "&&",
        "==",
        "===",
        "!=",
        "->",
        "%",
        "[+]",
        "[-]",
        "(o)",
        ">=",
    ],
    projects: [
        "( -_-)",
        "(-_•)",
        "( •_•)",
        "┌┐",
        "└┐",
        "⎾⏋",
        "⎿⏌",
        "[@_@]",
        "(>_<)",
        "o_o",
        "ಠ_ಠ",
        "^‿^",
        "[•-•]",
        "[+_+ ]",
        "░",
        "▒",
        "▓",
        ":)",
        "o7",
    ],
    education: [
        "Σ",
        "Δ",
        "Φ",
        "Ω",
        "π",
        "∫",
        "√",
        "∞",
        "∠",
        "⊥",
        "∥",
        "Ξ",
        "Ψ",
        "Λ",
        "I",
        "II",
        "III",
        "IV",
        "Æ",
        "x",
        "y",
        "z",
        "MCDXCII",
    ],
    skills: [
        "01",
        "10",
        "11",
        "00",
        "0_0",
        "8.8",
        "255",
        "1",
        "0",
        "0x",
        "¡!",
        "??",
        "!!",
        "::",
        ">>>",
        "<<<",
        "10100111001",
    ],
};

let objects = $state(
    Array.from({ length: 50 }, (_, i) => ({
        id: i,
        left: Math.random() * 100,
        delay: Math.random() * -30,
        duration: 10 + Math.random() * 15,
        size: 0.8 + Math.random() * 1.5,
        rotationSpeed: 5 + Math.random() * 10,
        reverse: Math.random() > 0.5,
        drift: (Math.random() - 0.5) * 500,
        pulseDuration: 3 + Math.random() * 12,
    })),
);

let currentChars = $derived(patterns[activeSection] || patterns.about);
</script>

<div class="bg-layer" aria-hidden="true">
    {#each objects as obj (obj.id)}
        <div 
            class="float-container"
            style:left="{obj.left}%"
            style:animation-delay="{obj.delay}s"
            style:animation-duration="{obj.duration}s"
            style="--drift: {obj.drift}px"
        >
            <span 
                class="ascii-art"
                class:reverse={obj.reverse}
                style:font-size="{obj.size}rem"
                style:animation-duration="{obj.rotationSpeed}s, {obj.pulseDuration}s"
            >
                {currentChars[obj.id % currentChars.length]}
            </span>
        </div>
    {/each}
</div>

<style>
    .bg-layer {
        position: fixed;
        inset: 0;
        z-index: -1;
        overflow: hidden;
        pointer-events: none;
        background: #111;
    }

    .float-container {
        position: absolute;
        top: -10%;
        animation: fall linear infinite;
    }

    .ascii-art {
        display: inline-block;
        color: #333;
        font-family: 'Courier New', monospace;
        white-space: pre;
        animation: spin linear infinite, pulse ease-in-out infinite;
        transition: color 0.8s ease, transform 0.2s ease;
    }

    .ascii-art.reverse {
        animation-direction: reverse, normal;
    }

    @keyframes fall {
        from { transform: translate(0, -10vh); }
        to { transform: translate(var(--drift), 110vh); }
    }

    @keyframes spin {
        from { rotate: 0deg; }
        to { rotate: 360deg; }
    }

    @keyframes pulse {
        0%, 100% {
            scale: 0.9;
            color: #222;
        }
        50% {
            scale: 1.4;
            color: #444;
        }
    }
</style>
