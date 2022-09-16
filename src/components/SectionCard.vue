<template>
  <div class="section__item">
    <div class="section__item__column">
      <span class="section__item__amount">{{
        props.data.title === "Average Age" ||
        props.data.title === "Average Membership"
          ? "~" + props.data.amount
          : props.data.amount
      }}</span>
      <p class="section__item__title">{{ props.data.title }}</p>
    </div>
    <div class="section__item__column">
      <FlagIcon v-if="props.data.icon === 'flag'" class="section__item__icon" />
      <MaleIcon
        v-else-if="props.data.icon === 'male'"
        class="section__item__icon"
      />
      <FemaleIcon
        v-if="props.data.icon === 'female'"
        class="section__item__icon"
      />
      <AgeIcon
        v-else-if="props.data.icon === 'age'"
        class="section__item__icon"
      />
      <UserIcon
        v-else-if="props.data.icon === 'user'"
        class="section__item__icon"
      />
    </div>
  </div>
</template>

<script setup>
import FlagIcon from "./icons/FlagIcon.vue";
import MaleIcon from "./icons/MaleIcon.vue";
import FemaleIcon from "./icons/FemaleIcon.vue";
import AgeIcon from "./icons/AgeIcon.vue";
import UserIcon from "./icons/UserIcon.vue";

const props = defineProps({
  data: {
    type: Object,
    required: true,
  },
});
</script>
