<script>
  import "@onsvisual/svelte-components/css/main.css";
  import "../app.css";
  import { Header, Main, Footer } from "@onsvisual/svelte-components";
</script>

<Header compact />
<Main>
  <slot />
</Main>
<Footer compact />
