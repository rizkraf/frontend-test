<template>
  <table class="table">
    <tr v-for="item in props.user" :key="item.login.uuid" class="table__user">
      <td>
        <img
          :src="item.picture.thumbnail"
          class="table__user__thumbnail"
          :alt="item.name.first"
        />
      </td>
      <td>
        <p class="table__user__name">
          {{ item.name.first + " " + item.name.last }}
        </p>
        <p class="table__user__email">{{ item.email }}</p>
      </td>
      <td>
        <p class="table__user__age">{{ item.dob.age }}</p>
      </td>
      <td>
        <span
          :class="
            item.gender === 'male'
              ? 'table__user__gender--male'
              : 'table__user__gender--female'
          "
          >{{ item.gender }}</span
        >
      </td>
      <td>
        <img
          :src="`https://flagcdn.com/20x15/${item.nat.toLowerCase()}.png`"
          class="table__user__nationality"
          alt=""
        />
      </td>
      <td>
        <p class="table__user__address">
          {{
            `${item.location.street.name} ${item.location.street.number}, ${item.location.city}, ${item.location.country}`
          }}
        </p>
      </td>
    </tr>
  </table>
</template>

<script setup>
const props = defineProps({
  user: {
    type: Array,
    required: true,
  },
});
</script>
