<script>
    export let href;
</script>

<div class="inline-block px-2 py-8 my-auto">
    <a {href} class="transition duration-300 bg-none hover:bg-primaryHover active:outline-none focus:outline-none text-white font-dosis text-4xl rounded-2xl p-3 font-normal">
        <slot></slot>
    </a>
</div>
