<template>
    <v-app>
        <v-jumbotron color="primary" dark height="200px">
            <v-container fill-height>
                <v-layout align-center>
                    <v-flex text-xs-left>
                        <h3 class="display-3">{{ title }}</h3>
                        <span class="subheading">{{ subheading }}</span>
                        <v-divider class="my-3"></v-divider>
                    </v-flex>
                </v-layout>
            </v-container>
        </v-jumbotron><br>

        <v-layout row wrap>
            <v-flex xs6 offset-xs3>
                <v-card>
                    <v-layout row>
                        <v-flex xs6 text-xs-left>
                            <v-dialog v-model="dialog" max-width="500px">
                                <v-btn slot="activator" color="primary" dark class="mb-2">Create Group</v-btn>
                                <v-card>
                                    <v-card-title>
                                        <span class="headline">{{ formTitle }}</span>
                                    </v-card-title>

                                    <v-card-text>
                                        <v-container grid-list-md>
                                            <v-layout wrap>
                                                <v-flex>
                                                    <v-text-field v-model="editedItem.name" label="Group name"></v-text-field>
                                                </v-flex>
                                            </v-layout>
                                        </v-container>
                                    </v-card-text>

                                    <v-card-actions>
                                        <v-spacer></v-spacer>
                                        <v-btn color="blue darken-1" flat @click.native="close">Cancel</v-btn>
                                        <v-btn color="blue darken-1" flat @click.native="save">Save</v-btn>
                                    </v-card-actions>
                                </v-card>
                            </v-dialog>
                        </v-flex>

                        <v-flex xs6 text-xs-right>
                            <v-btn color="success" @click="shuffleArray(groups)" v-show="groups.length > 2">Shuffle!</v-btn>
                        </v-flex>
                    </v-layout>
                </v-card>
                <v-card>
                    <v-container fluid grid-list-md>
                        <v-flex v-for="(item, index) in groups" :key="index">
                            <v-card color="blue darken-1" dark>
                                <v-layout row>
                                    <v-flex xs2>
                                        <h1>{{ index + 1 }}</h1>
                                    </v-flex>

                                    <v-flex xs6>
                                        <h1>{{ item.name }}</h1>
                                    </v-flex>

                                    <v-flex xs4>
                                        <v-btn fab dark small color="warning" @click="editItem(item)">
                                            <v-icon dark>edit</v-icon>
                                        </v-btn>

                                        <v-btn fab dark small color="error" @click="deleteItem(item)">
                                            <v-icon dark>delete</v-icon>
                                        </v-btn>
                                    </v-flex>
                                </v-layout>
                            </v-card>
                            <br>
                        </v-flex>
                    </v-container>
                </v-card>
            </v-flex>
        </v-layout>
    </v-app>
</template>

<script>
  export default {
    data: () => ({
      title: 'Not Fancy Shuffle Group',
      subheading: 'This Application will shuffle group name which defined by user.',
      dialog: false,
      groups: [],
      editedIndex: -1,
      editedItem: {
        name: '',
      },
      defaultItem: {
        name: '',
      }
    }),

    computed: {
      formTitle () {
        return this.editedIndex === -1 ? 'Create Group' : 'Edit Group'
      }
    },

    watch: {
      dialog (val) {
        val || this.close()
      }
    },

    methods: {
      editItem (item) {
        this.editedIndex = this.groups.indexOf(item)
        this.editedItem = Object.assign({}, item)
        this.dialog = true
      },

      deleteItem (item) {
        const index = this.groups.indexOf(item)
        confirm('Are you sure you want to delete this item?') && this.groups.splice(index, 1)
      },

      close () {
        this.dialog = false
        setTimeout(() => {
          this.editedItem = Object.assign({}, this.defaultItem)
          this.editedIndex = -1
        }, 300)
      },

      save () {
        if (this.editedIndex > -1) {
          Object.assign(this.groups[this.editedIndex], this.editedItem)
        } else {
          this.groups.push(this.editedItem)
        }
        this.close()
      },

      shuffleArray(array) {
        var counter = array.length, temp, name;

        while(counter > 0) {
          name = Math.floor(Math.random() * counter);

          counter--;

          temp = array[counter];
          array[counter] = array[name];
          array[name] = temp;
        }
        this.groups = [];
        this.groups = array;
      }
    }
  }
</script>
