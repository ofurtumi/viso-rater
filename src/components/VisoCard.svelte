<script lang="ts">
  export let title: string = "Test vísó";
  export let date: Date = new Date();
  export let seats: number = 50;
  export let taken_seats: number = 25;

  import face from "../assets/face.svg";
  import clock from "../assets/clock.svg";

  let stars = Array(5)
    .fill(null)
    .map((_) => false);

  const update_rating = (i: number) => {
    stars = stars.map((_, j) => (j <= i ? true : false));
  };
</script>

<div class="card">
  <h2>{title}</h2>

  <div class="stars">
    {#each stars as star, i}
      {#if star}
        <button on:click={() => update_rating(i)}>★</button>
      {:else}
        <button on:click={() => update_rating(i)}>☆</button>
      {/if}
    {/each}
  </div>

  <div class="info">
    <div class="time">
      <img src={clock} alt="Klukka" />
      <span>{date.getDate()}.{date.getMonth()}.{date.getFullYear()}</span>
    </div>
    <div class="seats">
      <span>{taken_seats}</span> / <span>{seats}</span>
      <img src={face} alt="Broskall" />
    </div>
  </div>
</div>

<style>
  .card {
    border: 2px solid #ccc;
    border-radius: 1rem;
    padding: 1rem;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    gap: 2rem;
  }

  h2 {
    margin: 0;
    flex: 1;
  }

  .stars {
    display: flex;
    gap: 1rem;
    justify-content: center;
  }

  .info {
    display: flex;
    justify-content: space-between;
  }

  .seats,
  .time {
    display: flex;
    align-items: center;
    gap: 0.5rem;
  }

  button {
    font-size: 2rem;
    background: none;
    border: none;
    cursor: pointer;
    margin: 0;
    padding: 0;
  }

  img {
    max-width: 1rem;
  }
</style>
