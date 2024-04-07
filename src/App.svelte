<script lang="ts">
  import VisoCard from "./components/VisoCard.svelte";

  interface Viso_Item {
    id: number;
    event_title: string;
    text: string;
    seats: number;
    taken_seats: number;
    startdate: Date;
    location: string;
    event_type: string;
  }

  type Viso_Data = Viso_Item[];

  let viso_data: Promise<Viso_Data> = get_data();

  async function get_data() {
    try {
      let raw_data = await fetch("https://nord.is/bakendi/feed");
      let data = await raw_data.json();
      data = data.feed.map((item: Viso_Item) => {
        item.startdate = new Date(item.startdate);
        item.taken_seats = Number(item.taken_seats);
        return item;
      });
      return data.filter((item: Viso_Item) => item.event_type === "Vísó");
    } catch (error) {
      throw error;
    }
  }
</script>

<main>
  {#await viso_data}
    <h1>Sæki gögn...</h1>
  {:then data}
    <h1>Vísó</h1>
    <section class="visoGrid">
      {#each data as item}
        <VisoCard
          title={item.event_title}
          date={item.startdate}
          seats={item.seats}
          taken_seats={item.taken_seats}
        />
      {/each}
    </section>
  {:catch error}
    <p class="error">Villa kom upp við að sækja gögn</p>
    <p class="error">{error}</p>
  {/await}
</main>

<style>
  main {
    width: 100%;
  }

  .error {
    color: red;
  }

  .visoGrid {
    width: 100%;
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
    gap: 1rem;
  }
</style>
