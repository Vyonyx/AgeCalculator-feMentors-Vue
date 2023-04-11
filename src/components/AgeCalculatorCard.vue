<script setup lang="ts">
import { ref } from 'vue';

const years = ref(0);
const months = ref(0);
const days = ref(0);
</script>

<template>
  <section class="card">

    <form>
      <div class="form-controls">
        <div class="form-control">
          <label for="day">DAY</label>
          <input required placeholder="DD" id="day" name="day" type="text" minlength="1" maxlength="2">
        </div>

        <div class="form-control">
          <label for="month">MONTH</label>
          <input required placeholder="MM" id="month" name="month" type="text" minlength="1" maxlength="2">
        </div>

        <div class="form-control">
          <label for="year">YEAR</label>
          <input required placeholder="YYYY" id="year" name="year" type="text" minlength="4" maxlength="4">
        </div>
      </div>

      <div class="submit-control">
        <button type="submit">
          <img src="../assets/icon-arrow.svg" alt="submit">
        </button>
      </div>
    </form>

    <h3 class="results-heading">
      <span class="results-number">{{ years || '- -' }}</span> years
    </h3>
    <h3 class="results-heading">
      <span class="results-number">{{ months || '- -' }}</span> months
    </h3>
    <h3 class="results-heading">
      <span class="results-number">{{ days || '- -' }}</span> days
    </h3>

  </section>
</template>

<style lang="scss">
$tablet-breakpoint: 768px;

.card {
  --spacing: 1.5rem;

  display: flex;
  flex-direction: column;

  background: var(--white);
  border-radius: 2rem;
  border-bottom-right-radius: 8rem;
  padding: var(--spacing);

  max-width: 650px;
  height: fit-content;
}

.form-controls {
  --grid-gap: 1rem;
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: var(--grid-gap);
  justify-content: space-between;
  align-items: center;
  max-width: 500px;

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
      font-size: 20px;
      font-weight: 800;

    }

    @media (min-width: $tablet-breakpoint) {
      input {
        font-size: 24px;
      }
    }
  }
}

.submit-control {
  --button-height: 4rem;

  position: relative;
  margin: 2rem 0;
  height: var(--button-height);

  &::before {
    --line-height: 1px;
    position: absolute;
    top: 50%;
    content: '';
    height: var(--line-height);
    background: var(--light-grey);
    width: 100%;
  }

  button {
    position: absolute;
    right: calc(50% - calc(var(--button-height)/2));
    height: var(--button-height);
    aspect-ratio: 1/1;
    border-radius: 100%;
    color: var(--white);
    background: var(--purple);
    border: none;
    cursor: pointer;


    img {
      width: 60%;
      aspect-ratio: 1/1;
    }
  }

  @media (min-width: $tablet-breakpoint) {
    --button-height: 5rem;

    button {
      right: 0;
    }
  }
}

.results-heading {
  margin: 0;
  font-size: 55px;
  font-weight: 800;
  font-style: italic;

  .results-number {
    color: var(--purple);
  }

  @media (min-width: $tablet-breakpoint) {
    font-size: 80px;
  }
}
</style>
