<!--COPIED FROM latest.svelte-->
<script>
  import { SERVER_URL } from "../../utils";
  import Navbar from "../../Components/Navbar.svelte";
  import { metatags } from "@roxi/routify";

  export let id;
  import "../../css/Latest.css";
  import "../../css/Skeletal.css";
  metatags.title = "Search results for " + '"' + decodeURIComponent(id) + '"';

  let latestPromise = new Promise((resolve, reject) => {
    fetch(`${SERVER_URL}/api/search`, {
      method: "POST",
      headers: {
        Accept: "application/json",
        "Content-Type": "application/json",
      },
      body: JSON.stringify({
        search: decodeURIComponent(id),
      }),
    }).then((res) => {
      if (res.ok) {
        res.json().then((data) => {
          resolve(data);
        });
      } else {
        res.json().then((data) => {
          reject(data);
        });
      }
    });
  });
</script>

<Navbar />
{#await latestPromise}
  <div class="latest-root">
    <h2 class="latest-text">Latest</h2>
    <br />
    <br />
    <div class="item-card-root">
      <div class="item skeleton">
        <div class="skeletal-image-small" />
        <br />
        <div class="skeleton-text" />
        <br />
        <div class="skeleton-text" />
      </div>
      <div class="item skeleton">
        <div class="skeletal-image-small" />
        <br />
        <div class="skeleton-text" />
        <br />
        <div class="skeleton-text" />
      </div>
      <div class="item skeleton">
        <div class="skeletal-image-small" />
        <br />
        <div class="skeleton-text" />
        <br />
        <div class="skeleton-text" />
      </div>
      <div class="item skeleton">
        <div class="skeletal-image-small" />
        <br />
        <div class="skeleton-text" />
        <br />
        <div class="skeleton-text" />
      </div>
      <div class="item skeleton">
        <div class="skeletal-image-small" />
        <br />
        <div class="skeleton-text" />
        <br />
        <div class="skeleton-text" />
      </div>
      <div class="item skeleton">
        <div class="skeletal-image-small" />
        <br />
        <div class="skeleton-text" />
        <br />
        <div class="skeleton-text" />
      </div>
      <div class="item skeleton">
        <div class="skeletal-image-small" />
        <br />
        <div class="skeleton-text" />
        <br />
        <div class="skeleton-text" />
      </div>
      <div class="item skeleton">
        <div class="skeletal-image-small" />
        <br />
        <div class="skeleton-text" />
        <br />
        <div class="skeleton-text" />
      </div>
      <div class="item skeleton">
        <div class="skeletal-image-small" />
        <br />
        <div class="skeleton-text" />
        <br />
        <div class="skeleton-text" />
      </div>
      <div class="item skeleton">
        <div class="skeletal-image-small" />
        <br />
        <div class="skeleton-text" />
        <br />
        <div class="skeleton-text" />
      </div>
    </div>
  </div>
{:then data}
  <div class="latest-root">
    <h2 class="latest-text">Search Results for "{decodeURIComponent(id)}"</h2>
    <br />
    <br />
    <div class="item-card-root">
      {#each data.data as item}
        <a href={`/watch/1/${item.id}`}>
          <div class="item">
            <img src={item.image} alt={item.id} class="item-img" />
            <br />
            <span class="item-name">{item.title}</span>
            <br />
            <span class="item-released">{item.released}</span>
          </div>
        </a>
      {/each}
    </div>
  </div>
{:catch}
  Error
{/await}
