<template>
  <v-item-group>
    <v-container>
      <v-row>
        <v-col
          v-for="(taskElement, index) in tasksList"
          :key="index"
          cols="12"
          md="4"
        >
          <v-item>
            <v-card :to="{name: 'EditTask', params: {id: taskElement.id}}" @click="addNewTaskList = true">
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
                <p class="task-status">
                  {{ taskElement.doneTask ? taskElement.doneTask : 0 }} /
                  {{ taskElement.allTask ? taskElement.allTask : 0 }}
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
export default {
  data () {
    return {
      addNewTaskList: false,
      newTaskListName: '',
      newTaskListDescription: '',
      newTaskRule: [v => (v || '').length > 0 || 'Pole Wymagane'],
      tasksList: [{
        id: 1,
        name: 'Nazwa  1',
        description: 'Opis lidasdssssssssssssssssssssssssssssssssssssssssssssssssty 1',
        allTask: 10,
        doneTask: 5
      }, {
        id: 2,
        name: 'Nazwa listy 2',
        description: 'Opis listy 1',
        tasks: [{
          name: 'zadanie 1.1',
          done: false
        }, {
          name: 'zadanie 1.2',
          done: false
        }]
      }, {
        id: 3,
        name: 'Nazwa listy 3',
        description: 'Opis listy 1',
        tasks: [{
          name: 'zadanie 1.1',
          done: false
        }, {
          name: 'zadanie 1.2',
          done: false
        }]
      }, {
        id: 4,
        name: 'Nazwa listy 4',
        description: 'Opis listy 1',
        tasks: [{
          name: 'zadanie 1.1',
          done: false
        }, {
          name: 'zadanie 1.2',
          done: false
        }]
      }, {
        id: 5,
        name: 'Nazwa listy 5',
        description: 'Opis listy 2',
        tasks: [{
          name: 'zadanie 2.1',
          done: false
        }, {
          name: 'zadanie 2.1',
          done: false
        }, {
          name: 'zadanie 2.1',
          done: false
        }, {
          name: 'zadanie 2.1',
          done: false
        }, {
          name: 'zadanie 2.1',
          done: false
        }, {
          name: 'zadanie 2.2',
          done: false
        }]
      }]
    }
  },

  computed: {
    newTaskValid () {
      return this.newTaskListName
    }
  },

  methods: {
    addTaskList () {
      if (this.newTaskListName !== '') {
        const lastTaskListId = this.tasksList.length === 0 ? 1 : this.tasksList[this.tasksList.length - 1].id
        this.tasksList.push({
          id: lastTaskListId + 1,
          name: this.newTaskListName,
          description: this.newTaskListDescription
        })
        this.newTaskListName = ''
        this.newTaskListDescription = ''
        this.addNewTaskList = false
      }
    },

    removeTaskList (taskListId) {
      this.tasksList = this.tasksList.filter(task => task.id !== taskListId)
      this.addNewTaskList = false
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
