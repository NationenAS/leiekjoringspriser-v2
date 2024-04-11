<script lang="ts">

// import { csv } from './assets/data';
// import { csvParse } from 'd3-dsv';

import data from './assets/data.json';

const categories = [
  {
    name: 'Traktor',
    image: 'enlp1.jpg'
  },
  {
    name: 'Jordarbeiding',
    image: 'enlp2.jpg'
  },
  {
    name: "Gjødsling, såing, og kalking",
    image: 'enlp3.jpg'
  },
  {
    name: 'Sprøyting',
    image: 'enlp4.jpg'
  },
  {
    name: 'Stråfôr- og grasarbeid',
    image: 'enlp5.jpg'
  },
  {
    name: 'Innhøsting',
    image: 'enlp6.jpg'
  },
  {
    name: 'Vedlikehold av skog og landskap',
    image: 'enlp7.jpg'
  },
  {
    name: 'Snørydding',
    image: 'enlp8.jpg'
  },
  {
    name: 'Anlegg, transport',
    image: 'enlp9.jpg'
  },
  {
    name: 'Diverse',
    image: 'enlp10.jpg'
  },
  {
    name: 'Soloutleie (ren arbeidskraft eller maskinutleie)',
    image: 'enlp11.jpg'
  }
];

</script>

<svelte:head>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin="true">
  <link href="https://fonts.googleapis.com/css2?family=Titillium+Web:wght@700&display=swap" rel="stylesheet">
</svelte:head>

<main>
  <div class="nav" id=nav>
    {#each categories as category}
    <a href="#{category.name}">{category.name}</a>
    {/each}
  </div>
  <div class="categories">
    {#each categories as category}
    <div class="category" id={category.name}>
      <div class="category-header" style="background-image: url(./{category.image})">
        <h2>
          <a href="#nav">Til toppen</a>
          {category.name}
        </h2>
      </div>
      <div class="list">
        <div class="row row-header">
          <div class="type"></div>
          <div class="values">
            <div></div>
            <div class="value">
              Laveste
            </div>
            <div class="value">
              Høyeste
            </div>
          </div>
        </div>
        {#each data.filter(d => d.Kategori === category.name) as item}
          <div class="row">
            <div class="type">
              {item.Type}
            </div>
            <div class="values">
              {#if item.Laveste == 'Timepris som oppdrag'}
              <div class="value span-2">
                {item.Laveste}
              </div>
              {:else}
              <div class="value unit">
                {item.Enhet}
              </div>
              <div class="value">
                <strong class="green">{item.Laveste}</strong> 
              </div>
              <div class="value">
                <strong class="red">{item.Høyeste}</strong>
              </div>
              {/if}
            </div>
          </div>
        {/each}
      </div>
    </div>
  {/each}
  </div>
</main>

<style>
  main {
    --padding: 1.8rem;
  }
  .categories {
    display: flex;
    flex-direction: column;
    gap: 2rem;
  }
  .category {
    border-radius: 5px;
    overflow: hidden;
    background: white;
  }
  .category-header {
    height: 20rem;
    background-size: cover;
    background-position: center;
    position: relative;
  }
  h2 {
    position: absolute;
    background: #355c77;
    padding: 0.6rem 1rem;
    left: var(--padding);
    bottom: 0;
    margin: 0;
    transform: translateY(50%);
    color: white;
    font-family: 'Titillium Web', sans-serif;
    line-height: 1.3;
    border-radius: 2px;
  }
  h2 a {
    font-size: 0.8rem;
    display: block;
    margin-bottom: 0.4rem;
    color: white;
  }
  .list {
    margin-top: 1rem;
    padding: var(--padding);
  }
  .row {
    display: flex;
    justify-content: space-between;
    padding: 0.75rem 0;
    border-bottom: 1px solid #ccc;
  }
  .row:last-child {
    border-bottom: none;
  }
  .row-header {
    font-weight: 500;
  }
  .values {
    display: flex;
    gap: 1rem;
  }
  .value {
    text-align: right;
    width: 5rem;
  }
  .unit {
    font-weight: 100;
    width: 3rem;
  }
  .span-2 {
    width: 10rem;
  }
  .red {
    color: red;
  }
  .green {
    color: green;
  }
  strong {
    font-weight: 500;
  }
</style>