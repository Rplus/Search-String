<script>
  import { createEventDispatcher } from 'svelte';
  const dispatch = createEventDispatcher();

  import { historeUrls, savedUrl } from './stores.js';
  import { getPmName, urlCoCoCo, getPM, genDdex } from './u.js';

  function applyUrl(p) {
    dispatch('apply', {
      url: p,
    });
  }

  function itemTitle(url) {
    let o = urlCoCoCo(url);
    let ddex = (getPM(o.uid) && getPM(o.uid).ddex) || genDdex(o.uid);
    let n = getPmName(ddex, o.lang);
    return [n, ...url.split('&')].map(i => {
      let ii = i.split('=').reverse();

      return `<span class="i ib">
        <sup>${ii[1] || ''}</sup>
        ${ii[0] || ''}
      </span>`
    }).join('')
  }
</script>

<style>
  details {
    margin-left: 5vmin;
  }

  .li {
    display: flex;
    align-items: start;
    margin-top: .5em;
    margin-bottom: .5em;
  }

  .btn {
    margin: 0 1em 0 0;
  }

  :global(.i.ib) {
    margin: 0 .2em;
    font-size: smaller;
  }

  :global(sup) {
    color: #99c;
  }
  :global(.i.ib:first-child) {
    display: block;
    font-size: larger;
    text-indent: -.75em;
  }

  :global(sup:empty) {
    display: none;
  }

  :global(sup::after) {
    content: '/';
    margin-left: .1em;
    margin-right: -.2em;
  }
</style>

<details open>
  <summary>
    Saved items:
  </summary>

  {#each $savedUrl as url (url)}
    <div class="li">
      <button class="btn"
        on:click|preventDefault="{() => historeUrls.remove(url) }"
      >⨯</button>

      <a href={ '#' + url }
        on:click="{() => applyUrl(url) }"
      >{@html itemTitle(url) }</a>
    </div>
  {/each}
</details>

