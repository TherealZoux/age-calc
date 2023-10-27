<script setup>
import { ref, watch } from "vue";

const // inputs
  birthDay = ref("24"),
  birthMonth = ref("1"),
  birthYear = ref("2004"),
  // current Dates
  currentYear = ref(new Date().getFullYear()),
  currentMonth = ref(new Date().getMonth() + 1),
  currentDay = ref(new Date().getDate());

const // outputs
  daysDiff = ref("--"),
  monthsDiff = ref("--"),
  yearsDiff = ref("--"),
  error = ref({});

const calcYears = function () {
  yearsDiff.value = currentYear.value - birthYear.value;
  monthsDiff.value = currentMonth.value - birthMonth.value;
  daysDiff.value = currentDay.value - birthDay.value;
  // correct month and days
  if (monthsDiff.value < 0) {
    yearsDiff.value--;
    monthsDiff.value += 12;
    daysDiff.value += 30;
  }
  if (daysDiff.value < 0) {
    monthsDiff.value--;
    daysDiff.value += 30;
  }
};

const validateForm = () => {
  error.value = {};

  if (!birthDay.value) {
    error.value.day = "This field is required";
  }
};
</script>

<template>
  <main>
    <form name="form" class="form" @submit.prevent="validateForm()">
      <div> 
        <label for="birth-day" :class="{ error: e }">DAY</label>
        <input
          name="birth-day"
          type="text"
          inputmode="numeric"
          placeholder="DD"
          min="1"
          max="31"
          :class="{ error: input.error }"
          v-model="birthDay"
        />
        <p v-if="error.day" :class="{ error: error.day }">
          {{ error.day }}
        </p>
      </div>
      <div>
        <label for="birth-month">MONTH</label>
        <input
          type="text"
          name="birth-month"
          inputmode="numeric"
          placeholder="MM"
          min="1"
          max="12"
          v-model="birthMonth"
        />
        <p></p>
      </div>
      <div>
        <label for="birth-year">YEAR</label>
        <input
          type="text"
          name="birth-year"
          inputmode="numeric"
          placeholder="YYYY"
          min="1800"
          max="2023"
          v-model="birthYear"
        />
        <p></p>
      </div>
    </form>
    <div>
      <button class="submit" type="submit" @click="validateForm()">
        <img src="./assets/icon-arrow.svg" alt="" />
      </button>
    </div>
    <div class="outputs">
      <h1>
        <span>{{ yearsDiff }}</span> <i>years</i>
      </h1>
      <h1>
        <span>{{ monthsDiff }}</span> <i>months</i>
      </h1>
      <h1>
        <span>{{ daysDiff }}</span> <i>days</i>
      </h1>
    </div>
  </main>
</template>

<style scoped>
form {
  display: flex;
  gap: 5rem;
}
form div {
  width: 6rem;
  display: flex;
  flex-direction: column;
}
form div label {
  font-size: 14px;
  font-weight: 700;
  color: var(--Smokey-grey);
  letter-spacing: 2px;
  margin-bottom: 6px;
}
form div input {
  width: 9rem;
  height: 4rem;
  padding: 18px;
  border: 2px solid var(--Off-white);
  border-radius: 12px;
  font-size: 27px;
  font-weight: 800;
  font-family: "Poppins", sans-serif;
  outline: none;
}

form div input::placeholder {
  color: var(--Smokey-grey);
  font-size: 30px;
  font-weight: 800;
  font-family: "Poppins", sans-serif;
}
form div input:focus {
  border: 1px solid var(--Purple);
}
.submit {
  background: var(--Purple);
  border-radius: 50%;
  padding: 1rem;
  border: none;
  float: inline-end;
}

.submit::after {
  content: "";
  width: 36rem;
  height: 2px;
  display: flex;
  position: absolute;
  background: var(--Off-white);
  left: 2rem;
  top: 10rem;
}
.submit:hover {
  background: var(--Off-black);
  cursor: pointer;
}
</style>
