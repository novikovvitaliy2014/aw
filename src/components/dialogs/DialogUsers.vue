<template>
  <v-layout row justify-start class="dialog" >
    <v-dialog v-model="dialog" max-width="350px">
      <template v-slot:activator="{ on }">
        <v-btn v-on="on" small dark color="green" @click="clearFields" class="add-button">Add User
        </v-btn>
      </template>
      <v-card>
        <v-card-title>
          <span class="headline">User Profile</span>
        </v-card-title>
        <v-card-text>
          <v-container grid-list-md>
            <v-layout wrap>
              <v-flex xs12>
                <v-text-field
                   label="Full name*"
                   v-model="addName"
                   :rules="nameRules">
                </v-text-field>
              </v-flex>
              <v-flex xs12>
                <v-text-field
                   label="Email*"
                   v-model="addEmail"
                   type="email"
                   :rules="emailRules">
                </v-text-field>
              </v-flex>
              <v-flex xs12 sm6>
                <v-select
                  :items="['Active', 'inActive']"
                  label="Activation"
                  v-model="addStatus"
                ></v-select>
              </v-flex>
            </v-layout>
          </v-container>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="green darken-1" flat @click="dialog = false">Close</v-btn>
          <v-btn color="green darken-1" flat @click="onSaveAdd">Save</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-layout>
</template>

<script>
  export default {
    data () {
      return {
        dialog: false,
        addName: '',
        addEmail: '',
        addStatus: '',
        emailRules: [
          v => !!v || 'Enter your email',
          v => /.+@.+/.test(v) || 'Enter a valid email address',
          v => (v && v.length >= 4) || 'Enter a valid email address'
        ],
         nameRules: [
          v => !!v || 'Enter your name',
          v => (v && v.length >= 3) || 'Name must be at least 3 characters'
        ],
      }
    },
    methods: {
      onSaveAdd() {
        if (this.addName.trim() === '' || this.addEmail.trim() === '' || this.addStatus === ''){
          return
        }
        this.dialog = false
        this.$store.dispatch("addUsersData", {
          name: this.addName,
          email: this.addEmail,
          status: this.addStatus
        })
        this.$emit('upgradeUsers');
      },
      clearFields(){
        this.addName = '',
        this.addEmail = '',
        this.addStatus = ''
      }
    }
  };
</script>
