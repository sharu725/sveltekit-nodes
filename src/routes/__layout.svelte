<script context="module">
  const modules = import.meta.glob("./**.svelte");

  let body = [];
  for (let path in modules) {
    let pathSanitized = path.replace(".svelte", "").replace("./", "/");
    body.push({
      title: pathSanitized.substring(pathSanitized.lastIndexOf("/") + 1),
      link: pathSanitized.includes("index")
        ? pathSanitized.replace("index", "")
        : pathSanitized,
    });
  }

  export const load = async () => {
    const menu = await Promise.all(body);
    return {
      props: {
        menu,
      },
    };
  };
</script>

<script>
  export let menu;
</script>

<ul>
  {#each menu as item}
    <li>
      <a href={item.link}>{item.title}</a>
    </li>
  {/each}
</ul>

<slot />
