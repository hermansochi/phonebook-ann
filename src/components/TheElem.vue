<script setup>
import { useEmplStore } from ".././stores/EmplStore";
import { mapState } from "pinia";
// import { storeToRefs } from "pinia";
import { RouterLink } from "vue-router";

import IconDelete from "./IconDelete.vue";
import IconFullinfo from "./IconFullinfo.vue";

// const { employeesData } = storeToRefs(useEmplStore());
const employeesData = useEmplStore(); // eslint-disable-line

const props = defineProps({ // eslint-disable-line
  employee: {
    id: { type: Number, required: true },
    cn: { type: String, required: true },
    title: { type: String, required: true },
    email: { type: String, required: true },
    telephone: { type: String, required: true },
    mobile: { type: String, required: true },
  },
});
</script>

<template>
  <ul class="container_employee">
    <li class="photo"></li>

    <li class="full_name">
      {{ employee.cn }}
    </li>

    <li class="position">{{ employee.title }}</li>
    <li class="email">{{ employee.email }}</li>

    <!-- форматинование номера геттерами-->
    <li class="phone">{{ getEmplTelephone(employee.id) }}</li>
    <li class="mobile">{{ getEmplMobile(employee.id) }}</li>

    <li class="employee_status" :class="{ active: !employee.hide }">
      {{ getEmplStatus }}
    </li>
    <li>
      <RouterLink
        :to="{
          name: 'card',
          params: { id: employee.id },
        }"
      >
        <button type="button" class="btn_fullInfo">
          <IconFullinfo />
        </button>
      </RouterLink>
    </li>
    <li>
      <button
        type="button"
        class="btn_delete"
        @click="this.$emit('delEmpl', this.employee)"
      >
        <IconDelete />
      </button>
    </li>
  </ul>
</template>

<script>
export default {
  computed: {
    // использую mapState (раздел без setup() в конце страницы) по https://pinia.vuejs.org/core-concepts/getters.html#with-setup
    ...mapState(useEmplStore, ["getEmplTelephone", "getEmplMobile"]),

    getEmplStatus() {
      return this.employee.hide == true ? "online" : "offline";
    },
  },
};
</script>

<style scoped>
.container_employee {
  display: grid;
  grid-template-columns: 5% 20% 10% 15% auto auto 9% 3% 4%;
  grid-gap: 2%;
  max-width: 100%;
  line-height: 1.6;
  align-items: center;
  list-style: none;
  border-radius: 8px;
  background-color: var(--vt-c-white-mute);
  padding: 10px;
}

.photo {
  width: 35px;
  height: 35px;
  background-color: var(--vt-c-grey-light);
  border-radius: 10%;
  margin: 0;
}

.full_name,
.position {
  text-overflow: ellipsis;
  overflow: hidden;
  white-space: nowrap;
}

.employee_status {
  display: flex;
  justify-content: center;
  width: 60px;
  color: var(--vt-c-white);
  font-size: 12px;
  line-height: 15px;
  opacity: 0.6;
  background-color: var(--vt-c-grey-silver);
  border-radius: 17px;
  padding: 3px 10px;
  margin: 0 auto;
}

.active {
  background-color: var(--vt-c-active-btn);
}
</style>
