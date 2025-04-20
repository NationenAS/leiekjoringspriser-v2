<script lang="ts">
import { onMount } from 'svelte';
import data from './assets/2025/data.json';

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

const getSubCategories = (category: string) => {
  return new Set(data.filter(d => d.Kategori === category).map(d => d.Underkategori));
}

onMount(() => {
  const setHeight = (attempt = 1, initialDelay = 0) => {
    const checkAndAdjustHeight = () => {
        const windowHeight = window.innerHeight;
        const documentHeight = document.body.offsetHeight + 360;

        console.log(`#${attempt} Window: ${windowHeight} Body: ${documentHeight}`);

      if (windowHeight != documentHeight) {
        window.parent.postMessage({ height: documentHeight }, "*");
        attempt++;
        const nextDelay = Math.pow(2, attempt + 1);
        setTimeout(() => {
          setHeight(attempt);
        }, nextDelay);
      }
    }
    if (initialDelay) {
      setTimeout(() => {
        checkAndAdjustHeight();
      }, initialDelay);
    } else {
      checkAndAdjustHeight();
    }
  }
  setHeight();
});

</script>

<svelte:head>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin="true">
  <link href="https://fonts.googleapis.com/css2?family=Titillium+Web:wght@700&display=swap" rel="stylesheet">
</svelte:head>

<main>
  <h1>Leiekjøringspriser</h1>
  <div class="intro">
    Her er vår komplette liste med priser på leiekjøring og soloutleie for gårdsdrift, snøbrøyting og anlegg/transport. Priser er oppdatert per april 2025.
  </div>
  <div class="nav" id=nav>
    {#each categories as category}
    <a href="#{category.name}">
      <div class="nav-card">
        <div class="nav-image">
          <img src="./{category.image}" alt={category.name}>
        </div>
        <div class="nav-title">{category.name}<div>
      </div>
    </a>
    {/each}
  </div>
  <div class="categories">
    {#each categories as category}
    {@const subCategories = getSubCategories(category.name)}
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
        {#each subCategories as subcat}
          {#if subcat !== category.name}
            <div class="row row-subcat">
              <div>{subcat}</div>
            </div>
          {/if}
          {#each data.filter(d => d.Kategori === category.name && d.Underkategori == subcat) as item}
            <div class="row">
              <div class="type">
                {item.Type}
              </div>
              {#if item.Laveste == 'Timepris som oppdrag'}
                <div class="unit"></div>
                <div class="values">
                  {item.Laveste}
                </div>
              {:else}
                <div class="unit">
                  {item.Enhet}
                </div>
                <div class="values">
                  <div class="value">
                    <div class="compact">Laveste</div>
                    <strong class="green">{item.Laveste}</strong> 
                  </div>
                  <div class="value">
                    <div class="compact">Høyeste</div>
                    <strong class="red">{item.Høyeste}</strong>
                  </div>
                </div>
              {/if}
            </div>
          {/each}
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
  h1 {
    font-family: 'Titillium Web', sans-serif;
    font-weight: 700;
    font-size: 2.5rem;
    margin-bottom: 1rem;
    margin-top: 1rem;
  }
  .intro {
    font-size: 1rem;
    margin-bottom: 2rem;
    max-width: 630px;
  }
  .nav {
    display: grid;
    gap: 1rem;
    grid-template-columns: repeat(auto-fill, minmax(15rem, 1fr));
    margin-bottom: 3rem;
  }
  .nav a {
    text-decoration: none;
    color: inherit;
  }
  .nav-card {
    background: white;
    height: 14rem;
    border-radius: 5px;
    overflow: hidden;
    display: flex;
    flex-direction: column;
    user-select: none;
  }
  .nav-card:hover {
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.15);
  }
  .nav-image {
    overflow: hidden;
    flex: 1;
  }
  .nav-image img {
    width: 100%;
    height: 100%;
    object-fit: cover;
    object-position: center;
    transition: 0.2s ease-out;
    user-select: none;
  }
  .nav-card:hover img {
    transform: scale(1.05);
  }
  .nav-title {
    padding: 0.5rem;
    font-family: 'Titillium Web', sans-serif;
    font-weight: 700;
    font-size: 1.1rem;
    text-align: center;
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
    padding: 0.7rem 1.2rem;
    left: var(--padding);
    bottom: 0;
    margin: 0;
    transform: translateY(50%);
    color: white;
    font-family: 'Titillium Web', sans-serif;
    line-height: 1.3;
    border-radius: 2px;
    max-width: calc(100% - (var(--padding)* 2));
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
    padding: 0.6rem 0;
  }
  .row-header {
    font-weight: 500;
  }
  .row-subcat {
    font-weight: 500;
    font-size: 1.05rem;
    margin-top: 0.75rem;
    text-decoration: underline;
    text-underline-offset: 0.2rem;
    text-decoration-thickness: 2px;
  }
  .type {
    flex: 1;
  }
  .values {
    display: flex;
    gap: 1rem;
  }
  .value,
  .unit {
    text-align: right;
  }
  .unit {
    font-weight: 100;
  }
  .value {
    min-width: 4rem;
  }
  .red {
    color: rgb(166, 14, 14);
  }
  .green {
    color: rgb(7, 96, 7);
  }
  strong {
    font-weight: 500;
  }
  .compact {
    display: none;
  }
  @media (max-width: 540px) {
    main {
      --padding: 1.2rem;
    }
    .list {
      margin-top: 2.5rem;
    }  
    .row {
      display: grid;
      grid-template-columns: 2fr 1fr;
      grid-template-rows: auto auto;
    }
    .row > :nth-child(3) {
      grid-row: 2;
      grid-column: span 2;
    }
    .row-header {
      display: none;
    }
    .row-subcat > div {
      grid-column: span 2;
    }
    .values {
      flex-direction: column;
      gap: 0.1rem;
      margin-top: 0.4rem;
    }
    .value {
      display: flex;
      justify-content: space-between;
    }
    .compact {
      display: block;
      font-weight: 100;
    }
  }
</style>