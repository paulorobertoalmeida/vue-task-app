<template>
  <form @submit.prevent="onSubmit" class="add-form">
    <div class="form-control">
      <label>Task</label>
      <input type="text" name="text" placeholder="Add Task" v-model="text" />
    </div>
    <div class="form-control">
      <label>Day & Time</label>
      <input
        type="text"
        name="day"
        placeholder="Add Day & Time"
        v-model="day"
      />
    </div>
    <div class="form-control form-control-check">
      <label>Set Reminder</label>
      <input
        type="checkbox"
        name="reminder"
        placeholder="Add Task"
        v-model="reminder"
      />
    </div>
    <input type="submit" value="Save Task" class="btn btn-block" />
  </form>
</template>

<script>
export default {
  name: "AddTask",
  data() {
    return {
      text: "",
      day: "",
      reminder: false,
    };
  },
  methods: {
    // 1st Iteration of function, usamos esta iteracion para imprimir los valores del formulario en consola para verificar, que nuestra funcion esta corriendo benne!
    // onSubmit() {
    //   if (!this.text) {
    //     alert("Yo yo yo, you best believe this aint gonna work homie! ");
    //     return;
    //   }
    //   const newTask = {
    //     id: Math.floor(Math.random() * 100),
    //     text: this.text,
    //     day: this.day,
    //     reminder: this.reminder,
    //   };
    //   console.log(newTask);
    //   (this.text = ""), (this.day = "");
    // },

    // 2nd Iteration of funciton, usamos esta iteracion para EMITIR $emit{} un customEvent que le pasaremos al papa que en este caso particular es ya el view/component HomeView.vue
    onSubmit() {
      if (!this.text) {
        alert("Yo yo yo, you best believe this aint gonna work homie! ");
        return;
      }
      const newTask = {
        // una vez, migramos a json-server el ID se genera automatico
        // id: Math.floor(Math.random() * 100),
        text: this.text,
        day: this.day,
        reminder: this.reminder,
      };
      this.$emit("add-task", newTask);
      (this.text = ""), (this.day = "");
    },
  },
};
</script>

<style scoped>
.add-form {
  margin-bottom: 40px;
}

.form-control {
  margin: 20px 0;
}

.form-control label {
  display: block;
}

.form-control input {
  width: 100%;
  height: 40px;
  margin: 5px;
  padding: 3px 7px;
  font-size: 17px;
}

.form-control-check {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.form-control-check label {
  flex: 1;
}

.form-control-check input {
  flex: 2;
  height: 20px;
}
</style>
