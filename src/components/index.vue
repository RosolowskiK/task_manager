<template>
  <v-item-group>
    <v-container>
      <v-row>
        <!--          Petla for do poruszania sie po stworzonych zadaniach-->
        <v-col
          v-for="(taskElement, index) in tasksList"
          :key="index"
          cols="12"
          md="4"
        >
          <v-item>
<!--          ustawienie :to pozwala przejść do widoku componentu view z parameter id tasku  -->
            <v-card :to="{name: 'view', params: {id: taskElement.id}}"  @click="addNewTaskList = true">
              <div class="cart-header">
                <v-card-title>
                  {{ taskElement.name }}
                  <v-spacer></v-spacer>
                  <v-btn
                    to="/"
                    depressed
                    color="error"
                    @click="removeTaskList(taskElement.id)"
                  >
                    Usuń
                  </v-btn>
                </v-card-title>
                <v-card-subtitle>
                  {{ taskElement.description }}
                </v-card-subtitle>
              </div>
              <v-card-text class="text-center">
                Status wykonanych zadań
                <p>
                  Wszystkie / Wykonane
                </p>
                <p class="task-status">
                  {{ taskElement.task_children_count ? taskElement.task_children_count : 0 }} /
                  {{ taskElement.task_children_done ? taskElement.task_children_done : 0 }}
                </p>
              </v-card-text>
            </v-card>
          </v-item>
        </v-col>

        <v-col
          cols="12"
          md="4"
          v-show="addNewTaskList"
        >
          <v-item>
            <v-card>
              <div class="cart-header">
                <v-card-title>
                  Nowa Lista
                </v-card-title>
              </div>
              <v-card-text>
<!--                vmodel ustawia wartosć zmiennej na co wprowadzimy w input-->
                <v-text-field v-model="newTaskListName" placeholder="Nazwa" :rules="newTaskRule"
                              required></v-text-field>
                <v-text-field v-model="newTaskListDescription" placeholder="Opis"></v-text-field>
              </v-card-text>
              <v-card-actions>
                <v-btn
                  :disabled="!newTaskValid"
                  @click="addTaskList"
                  class="success">
                  Zapisz
                </v-btn>
                <v-btn @click="addNewTaskList = false" class="error">
                  Anuluj
                </v-btn>
              </v-card-actions>
            </v-card>
          </v-item>
        </v-col>

        <v-col
          cols="12"
          md="4"
        >
          <v-item>
            <v-card>
              <div class="cart-header">
                <v-card-title>
                  Dodaj nowe zadanie
                </v-card-title>
              </div>
              <v-card-text class="text-center">
                <v-btn e @click="addNewTaskList = true" class="primary">
                  Dodaj
                </v-btn>
              </v-card-text>
            </v-card>
          </v-item>
        </v-col>
      </v-row>
    </v-container>
  </v-item-group>
</template>

<script>
import axios from 'axios'

export default {

  data () {
    return {
      addNewTaskList: false,
      newTaskListName: '',
      newTaskListDescription: '',
      newTaskRule: [v => (v || '').length > 0 || 'Pole Wymagane'],
      tasksList: [{
        id: 1,
        name: '',
        description: '',
        task_children_count: 0,
        task_children_done: 0
      }]
    }
  },

  created () {
    this.refreshTasksList()
  },

  computed: {
    newTaskValid () {
      return this.newTaskListName
    }
  },

  methods: {
    async removeTaskList (taskListId) {
      this.addNewTaskList = false
      await axios.delete('http://127.0.0.1:8000/task_root/' + taskListId + '/')
      await this.refreshTasksList()
    },

    async refreshTasksList () {
      const taskElementsApi = await axios.get('http://127.0.0.1:8000/task_root/')
      this.tasksList = taskElementsApi.data
    },

    async addTaskList () {
      if (this.newTaskListName !== '') {
        await axios.post('http://127.0.0.1:8000/task_root/', { name: this.newTaskListName, description: this.newTaskListDescription })
        this.addNewTaskList = false
        this.newTaskListDescription = ''
        this.newTaskListName = ''
        await this.refreshTasksList()
      }
    }

  }
}

</script>

<style>

.cart-header {
  background-color: #0003;
}

.task-status {
  font-size: 20px;
}

</style>
