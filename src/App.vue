<script setup>
import { ref, watch } from "vue";

const // inputs
  birthDay = ref(""),
  birthMonth = ref(""),
  birthYear = ref(""),
  // current Dates
  currentYear = ref(new Date().getFullYear()),
  currentMonth = ref(new Date().getMonth() + 1),
  currentDay = ref(new Date().getDate());

const // outputs
  daysDiff = ref("--"),
  monthsDiff = ref("--"),
  yearsDiff = ref("--"),
  error = ref({});

// get max dayes in this month
const daysInMonth = (month, year) => { // Use 1 for January, 2 for February, etc.
  return new Date(year, month, 0).getDate();
},
  maxDayes = daysInMonth(currentMonth.value, currentYear.value);


const calcYears = function () {
  yearsDiff.value = currentYear.value - birthYear.value;
  monthsDiff.value = currentMonth.value - birthMonth.value;
  daysDiff.value = currentDay.value - birthDay.value;
  // correct month and days
  birthMonth.value <= maxDayes 
  
  if (monthsDiff.value < 0) {
    yearsDiff.value--;
    monthsDiff.value += 12;
    daysDiff.value += 30;
  }
  if (daysDiff.value < 0) {
    monthsDiff.value--;
    daysDiff.value += maxDayes  ;
  }
};

const validateForm = () => {
  error.value = {};

  if (!birthDay.value ) {
    error.value.day = "This field is required";
  }
  else if(birthDay.value <1 || birthDay.value> maxDayes){
    error.value.day = "Not a valid date "
  }
  else if (birthMonth.value < 1 || birthMonth.value > 12) {
    error.value.month = "Not a valid date "
  }
  else if (birthYear.value < 1800 || birthYear.value > currentYear.value) {
    error.value.year = "Not a valid date "
  }
  else if (!birthMonth.value) {
    error.value.month = "This field is required";
  }
  else if (!birthYear.value) {
    error.value.year = "This field is required";
  }
  else{
    calcYears()
  }

};
</script>

<template>
  <main id="main">
    <form name="form" id="form" class="form" @submit.prevent="validateForm()">
      <div>
        <label for="birth-day" :class="{ error: error.day }">DAY</label>
        <input name="birth-day" id="input" type="number" inputmode="numeric"  placeholder="DD" min="1" max="31"
          :class="{ error: error.day }" v-model="birthDay"  required/>
        <p v-if="error.day" :class="{ error: error.day }">
          {{ error.day }}
        </p>
      </div>
      <div>
        <label for="birth-month" :class="{error: error.month}">MONTH</label>
        <input type="text" id="input" name="birth-month" inputmode="numeric" :class="{ error: error.month }"  placeholder="MM" min="1" max="12"
          v-model="birthMonth" required/>
        <p :class="{error: error.month}"> {{ error.month }}</p>
      </div>
      <div>
        <label for="birth-year" :class="{error: error.year}">YEAR</label>
        <input type="text" id="input" name="birth-year" inputmode="numeric" :class="{ error: error.year }" placeholder="YYYY" min="1800" max="2023"
          v-model="birthYear" required/>
        <p :class="{ error: error.year }">{{ error.year }}</p>
      </div>
      <div class="submit-div">
        <button class="submit" type="submit" @click="validateForm()">
          <img src="./assets/icon-arrow.svg" alt="" />
       </button>
      </div>
    </form>

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
input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}
input[type=number] {
  -moz-appearance: textfield;
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
  position: absolute;
  top: 6rem;
}

.submit::after {
  content: "";
  width: 33rem;
  height: 2px;
  display: flex;
  position: absolute;
  background: var(--Off-white);
  left: -33rem;
  top: 2.4rem;
}
.submit-div{
  position: relative;
}


.submit:hover {
  background: var(--Off-black);
  cursor: pointer;
}
/* error handle */
label.error {
  color: var(--Light-red);
}
input.error {
  color: var(--Light-red);
  border-color: var(--Light-red);
}
p.error {
  color: var(--Light-red);
  width: 8rem;
  font-size: 12px;
}


</style>