<script>
  import { goto, url } from "@roxi/routify";
  console.log("URL", window.location.pathname.split("/")[1]);

  import "../css/Navbar.css";
</script>

<header>
  <div class="navbar-root">
    <nav class="nav-item">
      <ul class="nav-area">
        <li>
          <div class="search-bar-root">
            <div class="search-bar-bgr">
              <div
                on:blur={(e) => {
                  if (e.target.innerText.length === 0) {
                    e.target.innerText = "Search Anime...";
                  }
                }}
                on:keypress={(e) => {
                  if (e.keyCode === 13) {
                    e.preventDefault();
                    location.replace(`/search/${e.target.innerText}`);
                  }
                }}
                contenteditable="true"
                class="search-bar-input"
                on:click={(e) => {
                  if (e.target.innerText === "Search Anime...") {
                    e.target.innerText = "";
                  }
                }}
              >
                Search Anime...
              </div>
            </div>
          </div>
        </li>
        {#if window.location.pathname.split("/")[1] === "home" || window.location.pathname.split("/")[1] === "latest" || window.location.pathname.split("/")[1] === ""}
          <li>
            <div class="links">
              <a href="/github">Github</a>
            </div>
          </li>
          <li>
            <div class="links"><a href="/donate">Donate</a></div>
          </li>
          <li>
            <div class="links"><a href="/popular">Popular</a></div>
          </li>
          <li>
            <div class="links current">
              <a href="/latest">Latest</a>
            </div>
          </li>
        {:else if window.location.pathname.split("/")[1] === "popular"}
          <li>
            <div class="links">
              <a href="/github">Github</a>
            </div>
          </li>
          <li>
            <div class="links"><a href="/donate">Donate</a></div>
          </li>
          <li>
            <div class="links current">
              <a href="/popular">Popular</a>
            </div>
          </li>
          <li>
            <div class="links "><a href="/latest">Latest</a></div>
          </li>
        {:else}
          <li>
            <div class="links">
              <a href="/github">Github</a>
            </div>
          </li>
          <li>
            <div class="links"><a href="/donate">Donate</a></div>
          </li>
          <li>
            <div class="links"><a href="/popular">Popular</a></div>
          </li>
          <li>
            <div class="links"><a href="/latest">Latest</a></div>
          </li>
        {/if}
      </ul>
    </nav>
  </div>
</header>
