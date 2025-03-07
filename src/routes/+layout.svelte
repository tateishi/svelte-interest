<script>
    let { children } = $props();
    import "../app.css";
</script>

<nav>
    <a class="text-orange-500" href="/">home</a>
    <a class="text-orange-500" href="/form1">form1</a>
    <a class="text-orange-500" href="/form2">form2</a>
</nav>
{@render children()}
