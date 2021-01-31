<template>
  <v-item-group>
    <v-container>
      <v-card>
        <div class="cart-header">
          <v-card-title>
            <div v-show="!editNameAndDescription" class="task-name">
              {{ taskElement.name }}
            </div>
            <v-text-field
              v-show="editNameAndDescription"
              v-model="newListName"
              :rules="newListNameRule"
              placeholder="Nazwa"
              label="Nazwa"
              outlined
            ></v-text-field>
            <v-spacer></v-spacer>
            <v-text-field
              v-show="editNameAndDescription"
              v-model="newListDescription"
              placeholder="Opis"
              label="Opis"
              outlined
            ></v-text-field>
            <v-spacer></v-spacer>
            <v-btn
              color="primary"
              v-show="!editNameAndDescription"
              @click="editTitleAndDescription"
            >
              Edytuj
            </v-btn>
            <v-btn
              v-show="editNameAndDescription"
              color="success"
              @click="saveNewTitleAndDescription"
              :disabled="!newListNameValid"
            >
              Zapisz
            </v-btn>
          </v-card-title>
          <v-card-subtitle v-show="!editNameAndDescription">
            {{ taskElement.description }}
          </v-card-subtitle>
        </div>
        <v-list>
          <v-list-item
            v-for="(task, index) in taskElement.task_children"
            :key="index"
          >
            <v-list-item-action>
              <v-checkbox v-model="task.done" @click="toggleTaskChildren(task.id, task.done)"></v-checkbox>
            </v-list-item-action>
            <v-list-item-content>
              <v-list-item-title :class="{'text-decoration-line-through': task.done}">
                {{ task.name }}
                <v-btn
                  depressed
                  color="error"
                  @click="removeTaskElement(task.id)"
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

import axios from 'axios'

export default {

  created () { // wywyołanie metody przy toworzeniu widoku
    this.refreshTasksList()
  },

  data () {
    return {
      newTaskElement: '',
      newListName: '',
      newListDescription: '',
      editNameAndDescription: false,
      taskRoot: this.$route.params, // podranie id z URL
      newListNameRule: [v => (v || '').length > 0 || 'Nazwa nie może być pusta'],
      taskElement: {
        id: 1,
        name: '',
        description: '',
        task_children: [{
          id: 1,
          name: '',
          done: false
        }]
      }
    }
  },

  computed: {
    newTaskValid () {
      return this.newTaskElement
    },

    newListNameValid () {
      return this.newListName
    }
  },

  methods: {
    async addNewTaskElement () {
      if (this.newTaskListName !== '') {
        await axios.post('http://127.0.0.1:8000/task_children/', {
          name: this.newTaskElement,
          task_root: this.taskRoot.id
        })
        this.newTaskElement = ''
        await this.refreshTasksList()
      }
    },

    async removeTaskElement (taskListId) {
      this.addNewTaskList = false
      await axios.delete('http://127.0.0.1:8000/task_children/' + taskListId + '/')
      await this.refreshTasksList()
    },

    editTitleAndDescription () {
      this.newListName = this.taskElement.name
      this.newListDescription = this.taskElement.description
      this.editNameAndDescription = true
    },

    async saveNewTitleAndDescription () {
      if (this.newListName !== '') {
        await axios.put('http://127.0.0.1:8000/task_root/' + this.taskRoot.id + '/', {
          name: this.newListName,
          description: this.newListDescription
        })
        this.editNameAndDescription = false
        await this.refreshTasksList()
      }
    },

    async toggleTaskChildren (taskChildrenId, taskDone) {
      await axios.put('http://127.0.0.1:8000/task_children/' + taskChildrenId + '/', {
        done: taskDone
      })
    },

    async refreshTasksList () {
      const taskElementsApi = await axios.get('http://127.0.0.1:8000/task_root/' + this.taskRoot.id + '/')
      this.taskElement = taskElementsApi.data
    }

  }
}
</script>
