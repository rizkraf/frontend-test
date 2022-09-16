<template>
  <h1 class="title">Member Dashboard</h1>

  <div class="section">
    <SectionCard v-for="item in data.section" :key="item.title" :data="item" />
  </div>

  <div class="table__wrapper">
    <SectionTable :user="data.user" />
  </div>
</template>

<script setup>
import { watchEffect, onMounted, reactive } from "vue";
import axios from "axios";
import SectionCard from "./components/SectionCard.vue";
import SectionTable from "./components/SectionTable.vue";

const data = reactive({
  section: [
    {
      amount: null,
      title: "Different Nationality",
      icon: "flag",
    },
    {
      amount: null,
      title: "Most Gender",
      icon: null,
    },
    {
      amount: null,
      title: "Average Age",
      icon: "age",
    },
    {
      amount: null,
      title: "Average Membership",
      icon: "user",
    },
  ],
  user: null,
});

const getUser = async () => {
  const response = await axios.get("https://randomuser.me/api/?results=25");
  try {
    const { results } = response.data;
    data.user = results;
  } catch (error) {
    console.log(error);
  }
};

watchEffect(async () => {
  if (data.user) {
    let tempNationality = [];
    let tempGender = [];
    let tempAge = [];
    let tempMembership = [];

    data.user.forEach(function (obj) {
      tempNationality.push(obj.nat);
      tempGender.push(obj.gender);
      tempAge.push(obj.dob.age);
      tempMembership.push(obj.registered.age);
    });

    const mostGender = tempGender.reduce(
      (acc, value) => ({
        ...acc,
        [value]: (acc[value] || 0) + 1,
      }),
      {}
    );

    const averageAge = tempAge.reduce((a, b) => a + b, 0) / tempAge.length;

    const averageMembership =
      tempMembership.reduce((a, b) => a + b, 0) / tempMembership.length;

    data.section[0].amount = [...new Set(tempNationality)].length;

    if (mostGender.male > mostGender.female) {
      data.section[1].amount = "male";
      data.section[1].icon = "male";
    } else {
      data.section[1].amount = "female";
      data.section[1].icon = "female";
    }

    data.section[2].amount = Math.floor(averageAge);

    data.section[3].amount = Math.floor(averageMembership) + " year";
  }
});

onMounted(() => {
  getUser();
});
</script>

<style lang="scss">
body {
  background-color: #f2f2fc;
  font-family: Arial, Helvetica, sans-serif;
}

h1,
h2,
h3,
h4,
h5,
p {
  margin: 0;
}

.title {
  font-size: 20px;
  margin: 1.5rem 0;
}

.section {
  display: grid;
  grid-template-columns: repeat(2, minmax(0, 1fr));
  gap: 1rem;

  &__item {
    display: flex;
    align-items: center;
    justify-content: space-between;
    background-color: #ffffff;
    padding: 1.5rem;
    border-radius: 10px;
    box-shadow: 2px 2px 12px 0px rgba(0, 0, 0, 0.1);
    -webkit-box-shadow: 2px 2px 12px 0px rgba(0, 0, 0, 0.1);
    -moz-box-shadow: 2px 2px 12px 0px rgba(45, 23, 23, 0.1);

    &__amount {
      text-transform: capitalize;
      font-size: 2rem;
      font-weight: 600;
    }

    &__title {
      font-size: 14px;
      margin: 0;
      color: #8e8e8e;
    }

    &__icon {
      height: 3rem;
      width: 3rem;
    }
  }
}

.table {
  width: 100%;
  border-spacing: 1rem;

  &__wrapper {
    height: 500px;
    overflow-y: auto;
    margin: 1.5rem 0;
  }

  &__user {
    &__thumbnail {
      height: 2rem;
      width: 2rem;
      border-radius: 100%;
    }

    &__gender {
      &--male,
      &--female {
        border-radius: 5px;
        padding: 3px 5px;
        color: #ffffff;
        font-weight: 600;
        font-size: 12px;
      }

      &--male {
        background-color: #92b9e3;
      }
      &--female {
        background-color: #fba0cf;
      }
    }

    &__email {
      color: #8e8e8e;
      font-size: 14px;
    }
  }
}
</style>
