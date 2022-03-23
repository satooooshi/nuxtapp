<template>
  <div class="container">
    <h1>Nuxt-ToDo-App</h1>
    <br />
    <h3>
      <form @submit.prevent="addTask()">
        Add Task : <input v-model="task" type="text" />
      </form>
      <br /><br />
      <table>
        <tbody>
          <tr v-for="(task, index) in tasks" :key="task.id">
            <td :class="{ done: task.isDone }">{{ task.name }}</td>
            <td
              v-if="task.isDone"
              class="pointer"
              @click="task.isDone = !task.isDone"
            >
              [済]
            </td>
            <td v-else class="pointer" @click="task.isDone = !task.isDone">
              [未]
            </td>
            <td class="pointer" @click="removeTask(index)">[x]</td>
            <br />
            <br />
          </tr>
        </tbody>
      </table>
    </h3>
  </div>
</template>

<script lang="ts">
import { defineComponent, reactive, toRefs } from '@nuxtjs/composition-api'

interface Task {
  name: string
  isDone: boolean
}

export default defineComponent({
  setup() {

    // state
    const state = reactive({
      task: '',
      tasks: [] as Task[],
    })

    const addTask = () => {
      const taskObj: Task = { name: state.task, isDone: false }
      state.tasks.push(taskObj)
      state.task = ''
    }

    const removeTask = (index: number) => {
      state.tasks.splice(index, 1)
    }

    return {
      ...toRefs(state),
      addTask,
      removeTask,
    }
  },
})
</script>

<style>
.container {
  text-align: center;
}

table {
  margin: 0 auto;
}

.done {
  text-decoration: line-through 3px;
}

.pointer {
  cursor: pointer;
}
</style>