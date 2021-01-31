<template>
  <v-item-group>
    <v-container>
      <v-card>
        <div class="cart-header">
          <v-card-title>
            {{ taskElement.name }}
            <v-spacer></v-spacer>
            <v-btn
              color="primary"
            >
              Edytuj
            </v-btn>
          </v-card-title>
          <v-card-subtitle>
            {{ taskElement.description }}
          </v-card-subtitle>
        </div>
        <v-list>
          <v-list-item
            v-for="(task, index) in taskElement.tasks"
            :key="index"
          >
            <v-list-item-action>
              <v-checkbox v-model="task.done"></v-checkbox>
            </v-list-item-action>
            <v-list-item-content>
              <v-list-item-title :class="{'text-decoration-line-through': task.done}">
                {{ task.name }}

                <v-btn
                  depressed
                  color="error"
                  @click="removeTask(task.id)"
                >
                  Usuń
                </v-btn>
              </v-list-item-title>
            </v-list-item-content>
          </v-list-item>
          <v-list-item>
            <v-list-item-content>
              <v-list-item-title>
                <v-text-field v-model="newTaskElement" placeholder="Nowe zadanie"></v-text-field>
                <v-btn
                  @click="addNewTaskElement"
                  :disabled="!newTaskValid"
                  class="primary"
                >
                  Dodaj
                </v-btn>
              </v-list-item-title>
            </v-list-item-content>
          </v-list-item>
        </v-list>
      </v-card>
    </v-container>
  </v-item-group>
</template>

<script>
export default {
  props: {
    newTaskElement: String,
    newListName: String,
    newListDescription: String
  },

  data () {
    return {
      editNameAndDescription: false,
      taskElement: {
        id: 5,
        name: 'Nazwa listy 5',
        description: 'Opis listy 2',
        tasks: [{
          id: 1,
          name: 'zadanie 2.1',
          done: false
        }, {
          id: 2,
          name: 'zadanie 2.2',
          done: false
        }, {
          id: 3,
          name: 'zadanie 2.3',
          done: false
        }, {
          id: 4,
          name: 'zadanie 2.4',
          done: false
        }, {
          id: 5,
          name: 'zadanie 2.5',
          done: false
        }, {
          id: 6,
          name: 'zadanie 2.6',
          done: false
        }, {
          id: 1,
          name: 'zadanie 2.1',
          done: false
        }, {
          id: 2,
          name: 'zadanie 2.2',
          done: false
        }, {
          id: 3,
          name: 'zadanie 2.3',
          done: false
        }, {
          id: 4,
          name: 'zadanie 2.4',
          done: false
        }, {
          id: 5,
          name: 'zadanie 2.5',
          done: false
        }, {
          id: 6,
          name: 'zadanie 2.6',
          done: false
        }]
      }
    }
  },

  computed: {
    newTaskValid () {
      return this.newTaskElement
    }
  },

  methods: {
    removeTask (id) {
      this.taskElement.tasks = this.taskElement.tasks.filter(task => task.id !== id)
    },

    addNewTaskElement () {
      if (this.newTaskElement !== '') {
        const lastTaskListId = this.taskElement.tasks.length === 0 ? 1 : this.taskElement.tasks[this.taskElement.tasks.length - 1].id
        this.taskElement.tasks.push({
          id: lastTaskListId + 1,
          name: this.newTaskElement
        })
        this.newTaskElement = ''
      }
    }
  }
}
</script>
