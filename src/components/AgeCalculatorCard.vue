<script setup lang="ts">
import { ref } from 'vue';
import type { Ref } from 'vue';
import { intervalToDuration } from 'date-fns';

let calculatedYears: Ref<number | null> = ref(null);
let calculatedMonths: Ref<number | null> = ref(null);
let calculatedDays: Ref<number | null> = ref(null);

const userInput = ref({
  year: '',
  month: '',
  day: '',
});

const errorState = ref({
  isError: false,
  year: '',
  month: '',
  day: '',
});

function handleInputChange(e: Event) {
  const input = e.target as HTMLInputElement;
  const id = input.id;

  if (id === 'year' || id === 'month' || id === 'day') {
    userInput.value[id] = input.value;
    errorState.value[id] = "";
  }

  if (userInput.value.year && userInput.value.month && userInput.value.day) {
    errorState.value.isError = false;
    errorState.value.year = "";
    errorState.value.month = "";
    errorState.value.day = "";
  }

}

function calculateAge() {
  try {
    const { year, month, day } = userInput.value
    if (!year || !month || !day) throw new Error("Required");

    const currentDate = new Date;
    const userDate = new Date(`${year}-${month}-${day}`);

    if (Number.parseInt(userInput.value.year) > currentDate.getFullYear()) errorState.value.year = "Must be in the past";
    if (Number.parseInt(userInput.value.month) > 12) errorState.value.month = "Must be a valid month";
    if (!userDate.getDay()) errorState.value.day = "Must be a valid day";

    if (errorState.value.year || errorState.value.month || errorState.value.day) throw new Error("Invalid");

    const difference = intervalToDuration({ start: userDate, end: currentDate });
    if (!difference) return;

    const { years, months, days } = difference;

    calculatedYears.value = years || null;
    calculatedMonths.value = months || null;
    calculatedDays.value = days || null;

  } catch (error) {
    if (error instanceof Error && error.message === "Required") {
      errorState.value.isError = true;
      Object.keys(errorState.value).forEach(key => {
        if (key === "year" || key === "month" || key === "day")
          errorState.value[key] = userInput.value[key] ? "" : "This field is required"
      })
    } else if (error instanceof Error && error.message === "Invalid") {
      errorState.value.isError = true;
    }
  }
}
</script>

<template>
  <section class="card">

    <form @submit.prevent="calculateAge">
      <div class="form-controls">
        <div class="form-control" :class="errorState.isError && 'form-error'">
          <label for="day">DAY</label>
          <input @change="handleInputChange" placeholder="DD" id="day" name="day" type="text" minlength="1" maxlength="2">
          <p v-if="errorState.isError" class="error-msg">{{ errorState.day }}</p>
        </div>

        <div class="form-control" :class="errorState.isError && 'form-error'">
          <label for="month">MONTH</label>
          <input @change="handleInputChange" placeholder="MM" id="month" name="month" type="text" minlength="1"
            maxlength="2">
          <p v-if="errorState.isError" class="error-msg">{{ errorState.month }}</p>
        </div>

        <div class="form-control" :class="errorState.isError && 'form-error'">
          <label for="year">YEAR</label>
          <input @change="handleInputChange" placeholder="YYYY" id="year" name="year" type="text" minlength="4"
            maxlength="4">
          <p v-if="errorState.isError" class="error-msg">{{ errorState.year }}</p>
        </div>
      </div>

      <div class="submit-control">
        <button type="submit">
          <img src="../assets/icon-arrow.svg" alt="submit">
        </button>
      </div>
    </form>

    <h3 class="results-heading">
      <span class="results-number">{{ calculatedYears || '- -' }}</span> years
    </h3>
    <h3 class="results-heading">
      <span class="results-number">{{ calculatedMonths || '- -' }}</span> months
    </h3>
    <h3 class="results-heading">
      <span class="results-number">{{ calculatedDays || '- -' }}</span> days
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

@media (min-width: $tablet-breakpoint) {
  .card {
    width: 100%;
  }
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
    position: relative;
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

      &:focus {
        outline: none !important;
        border-color: var(--purple);
      }
    }

    .error-msg {
      position: absolute;
      left: 0;
      top: calc(100% + 5px);
      font-size: 0.8rem;
    }

    @media (min-width: $tablet-breakpoint) {
      input {
        font-size: 24px;
      }
    }
  }

  .form-control.form-error {
    color: red;
    border-color: red;

    label {
      color: inherit;
    }

    input {
      border-color: inherit;
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
  color: var(--off-black);

  .results-number {
    color: var(--purple);
  }

  @media (min-width: $tablet-breakpoint) {
    font-size: 80px;
  }
}
</style>
