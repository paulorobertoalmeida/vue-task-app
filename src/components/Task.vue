<template>
  <div
    @click="$emit('toggle-reminder', id)"
    :class="[task.reminder ? 'reminder' : '', 'task']"
  >
    <h3>
      {{ task.text }}
      <i @click="$emit('delete-task', task.id)" class="fas fa-times"></i>
    </h3>
    <p>{{ task.day }}</p>
  </div>
</template>

<script>
export default {
  name: "Task",
  props: {
    task: Object,
  },
  methods: {
    // NOS DESPRENDEMOS DEL METODO-onDelete DENTRO DEL METODO{} ya uqe usaremos el $emit, que contiene un event custom dentro del htnl :)- WUTANG
    // onDelete(id) {
    //   //   console.log("Task Id", id);
    //   this.$emit("delete-task", id);
    // },
  },
  emits: ["delete-task", "toggle-reminder"],
};
</script>

<style scoped>
.fas {
  color: red;
  cursor: pointer;
}

.task {
  background: #f4f4f4;
  margin: 5px;
  padding: 10px 20px;
  cursor: pointer;
}

.task.reminder {
  border-left: 5px solid green;
}

.task h3 {
  display: flex;
  align-items: center;
  justify-content: space-between;
}
</style>
