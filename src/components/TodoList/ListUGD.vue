<template>
 <v-main class="list">
   <h3 class="text-h3 font-weight-medium mb-5">To Do List</h3>

   <v-card>
     <v-card-title>
       <v-text-field
         v-model="search"
         append-icon="mdi-magnify"
         label="Search"
         single-line
         hide-details
       ></v-text-field>
       <v-spacer></v-spacer>
       <v-btn color="success" dark @click="dialog = true">
         Tambah
       </v-btn>
     </v-card-title>

     <v-data-table :headers="headers" :items="todos" :search="search"  >
       <template v-slot:[`item.actions`]="{ item }">   
        <v-btn small @click="dialogShow = true">
           Note
         </v-btn>
         <v-btn small @click="editItem(item)">
           edit
         </v-btn>
         <v-btn small @click="deleteItem(item); dialogDelete=true">
           Delete
         </v-btn>
       </template>
     </v-data-table>
   </v-card>
    <!-- dialog note -->
    <!-- <v-dialog v-model="dialogShow" max-width="500px">
        <v-card>
            <v-card-text>
            <v-container>
                v-model="formTodo.task"

                v-model="formTodo.priority"
                :items="['Penting', 'Biasa', 'Tidak penting']"

                v-model="formTodo.note"
            </v-container>
       </v-card-text>
            <v-card-actions>
            <v-btn color="primary" text @click="dialogShow = false">Close</v-btn>
            </v-card-actions>
        </v-card>
    </v-dialog> -->
   <!-- dialog del -->
    <v-dialog v-model="dialogDelete" max-width="500px">
        <v-card>
            <v-card-title>Delete</v-card-title>
            <v-card-text>Apakah anda yakin menghapus?</v-card-text>
            <v-card-actions>
            <v-btn color="primary" text @click="dialogDelete = false">Close</v-btn>
            <v-btn color="primary" text @click="deleteItem(item); ">Delete</v-btn>
            </v-card-actions>
        </v-card>
    </v-dialog>

   <v-dialog v-model="dialog" persistent max-width="600px">
     <v-card>
       <v-card-title>
         <span class="headline">Form Todo</span>
       </v-card-title>
       <v-card-text>
         <v-container>
           <v-text-field
             v-model="formTodo.task"
             label="Task"
             required
           ></v-text-field>

           <v-select
             v-model="formTodo.priority"
             :items="['Penting', 'Biasa', 'Tidak penting']"
             label="Priority"
             required
           ></v-select>

           <v-textarea
             v-model="formTodo.note"
             label="Note"
             required
           ></v-textarea>
         </v-container>
       </v-card-text>
       <v-card-actions>
         <v-spacer></v-spacer>
         <v-btn color="blue darken-1" text @click="cancel">
           Cancel
         </v-btn>
         <v-btn color="blue darken-1" text @click="save">
           Save
         </v-btn>
       </v-card-actions>
     </v-card>
   </v-dialog>
 </v-main>
</template>
<script>
export default {
 name: "List",
 data() {
   return {
     search: null,
     filter:null,
     dialog: false,
     editedIndex: -1,
     dialogDelete: false,
     dialogShow: false,
     headers: [
       {
         text: "Task",
         align: "start",
         sortable: true,
         value: "task",
       },
       { text: "Priority", value: "priority" },
       { text: "Note", value: "note" },
       { text: "Actions", value: "actions" },
     ],
     todos: [
       {
         task: "bernafas",
         priority: "Penting",
         note: "huffttt",
       },
       {
         task: "nongkrong",
         priority: "Tidak penting",
         note: "bersama tman2",
       },
       {
         task: "masak",
         priority: "Biasa",
         note: "masak air 500ml",
       },
     ],
     formTodo: {
       task: null,
       priority: null,
       note: null,
     },
   };
 },
 methods: {
    deleteItem(item){
        const index = this.todos.indexOf(item)
        this.formTodo = Object.assign({}, item)
        this.dialogDelete = true
        this.todos.splice(index, 1)
    }
    ,
    showDeleteDialog(item) {
        this.itemToDelete = item
        this.dialogDelete = !this.dialogDelete
    },
    // showDialog(item) {
    //     const index = this.todos.indexOf(item)
    //     this.itemToShow = item
    //     this.dialogShow = !this.dialogShow
    // },
   save() {

       if (this.editedIndex > -1) {
          Object.assign(this.todos[this.editedIndex], this.formTodo)
        } else {
          this.todos.push(this.formTodo)
        }
        this.resetForm();
     this.dialog = false;
   },
   cancel() {
     this.resetForm();
     this.dialog = false;
   },
   resetForm() {
     this.formTodo = {
       task: null,
       priority: null,
       note: null,
     };
   },
       
   editItem(item){
        this.dialog = true
        this.editedIndex = this.todos.indexOf(item)
        this.formTodo = Object.assign({}, item)
    }
   },
};
</script>
