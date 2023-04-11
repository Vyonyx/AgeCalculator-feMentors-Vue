<script setup lang="ts">
</script>

<template>
  <section class="card">

    <form>
      <div class="form-control">
        <label for="day">DAY</label>
        <input id="day" name="day" type="text" min="1" max="31">
      </div>

      <div class="form-control">
        <label for="month">MONTH</label>
        <input id="month" name="month" type="text" min="1" max="12">
      </div>

      <div class="form-control">
        <label for="year">YEAR</label>
        <input id="year" name="year" type="text">
      </div>
    </form>

  </section>
</template>

<style lang="scss">
.card {
  --spacing: 1.5rem;

  display: flex;
  flex-direction: column;

  background: var(--white);
  border-radius: 2rem;
  border-bottom-right-radius: 8rem;
  padding: var(--spacing);

  max-width: 25rem;
  height: 50vh;
}

form {
  --grid-gap: 1rem;
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: var(--grid-gap);
  justify-content: space-between;
  align-items: center;

  .form-control {
    display: flex;
    flex-direction: column;
    gap: calc(var(--grid-gap) / 5);


    label {
      color: var(--smokey-grey);
      font-size: 11px;
      font-weight: 800;
      letter-spacing: 0.15rem;
      width: min-content;
    }

    input {
      --height: 2.5rem;
      padding: calc(var(--grid-gap)/2) var(--grid-gap);
      height: var(--height);
      border: 1px solid var(--light-grey);
      border-radius: 0.5rem;
      width: calc(100% - calc(var(--grid-gap) + var(--grid-gap)));
      font-size: 24px;
      font-weight: 800;
    }
  }
}
</style>
