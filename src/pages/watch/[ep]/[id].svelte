<script>
  import Navbar from "../../../Components/Navbar.svelte";
  import { SERVER_URL } from "../../../utils";
  import {
    params,
    goto,
    redirect,
    afterPageLoad,
    metatags,
  } from "@roxi/routify";

  import "../../../css/Watch.css";
  import "../../../css/Skeletal.css";
  const { ep, id } = $params;

  const watchPromise = new Promise(async (resolve, reject) => {
    const data = {};
    console.log("ID", id);
    fetch(`${SERVER_URL}/api/anime/data`, {
      method: "POST",
      headers: {
        Accept: "application/json",
        "Content-Type": "application/json",
      },
      body: JSON.stringify({
        id: decodeURIComponent(id),
      }),
    }).then((r) =>
      r.json().then((e) => {
        data.data = e;
        fetch(`${SERVER_URL}/api/stream`, {
          method: "POST",
          headers: {
            Accept: "application/json",
            "Content-Type": "application/json",
          },
          body: JSON.stringify({
            id: decodeURIComponent(id),
            ep: ep,
          }),
        }).then((r) =>
          r.json().then((e) => {
            data.episodes = e;
            document.title = `${data.data.data.title} - EP ${ep}`;
            resolve(data);
            console.log(data);
          })
        );
      })
    );
  });
</script>

<Navbar />

{#await watchPromise}
  <div class="watch-root">
    <div class="right-side">
      <h2 class="anime-title">
        <span class="blue">Loading...</span>
      </h2>
      <div class="skeletal-iframe" />
      <div class="episodes skeleton" />
    </div>
    <div class="left-side">
      <div class="skeleton-text" />
      <div>
        <div class="skeleton-text" />
      </div>

      <div>
        <div class="skeleton-text" />
      </div>

      <div>
        <div class="skeleton-text" />
      </div>

      <div>
        <div class="skeleton-text" />
      </div>

      <div>
        <div class="skeleton-text" />
      </div>

      <div>
        <div class="skeleton-text" />
      </div>
    </div>
  </div>
{:then anime}
  <div class="watch-root">
    <div class="right-side">
      <h2 class="anime-title">
        Watching <span class="blue">{anime.data.data.title}, Episode {ep}</span>
      </h2>
      {#if anime.data.data.episodes.length === 0}
        <img
          class="anime-image"
          src={anime.data.data.image}
          alt={anime.data.data.title}
        />
      {/if}
      <iframe
        src={anime.episodes.data}
        frameborder="0"
        title="player"
        class="player-iframe"
        allowfullscreen="true"
        scrolling="no"
      />
      <div class="episodes">
        {#if anime.data.data.episodes.length === 0}
          <span style="font-family: Poppins;">No eps Found </span>
        {/if}
        {#each anime.data.data.episodes as ep, i}
          {#if i + 1 === ep}
            <div
              class="episode-box current"
              on:click={() => {
                window.location.replace(`/watch/${i + 1}/${id}`);
              }}
            >
              {`${ep.name} ${ep.cate}`}
            </div>
          {:else}
            <div
              class="episode-box"
              on:click={() => {
                window.location.replace(`/watch/${i + 1}/${id}`);
              }}
            >
              {`${ep.name} ${ep.cate}`}
            </div>
          {/if}
        {/each}
      </div>
    </div>
    <div class="left-side">
      <div><h2>{anime.data.data.title}</h2></div>
      <div>
        <span class="green">Type: </span>
        {`${anime.data.data.type}`.split(":")[1]}
      </div>

      <div>
        <span class="green">Desc: </span>
        {`${anime.data.data.desc}`.split(":")[1]}
      </div>

      <div>
        <span class="green">Genre: </span>
        {`${anime.data.data.type}`.split(":")[1]}
      </div>

      <div>
        <span class="green">Released: </span>
        {`${anime.data.data.releaased}`.split(":")[1]}
      </div>

      <div>
        <span class="green">Status: </span>
        {`${anime.data.data.status}`.split(":")[1]}
      </div>

      <div>
        <span class="green">Other names: </span>
        {`${anime.data.data.othername}`.split(":")[1]}
      </div>
    </div>
  </div>
{/await}
