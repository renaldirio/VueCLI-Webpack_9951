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
        <!-- warna -->
        <template v-slot:[`item.priority`]="{ item }">
        <td>
        <v-chip v-if="item.priority == 'Penting'" color="red" label outlined>
            {{ item.priority }}
        </v-chip>
        <v-chip v-else-if="item.priority == 'Biasa'" color="success" label outlined>
            {{ item.priority }}
        </v-chip>
        <v-chip v-else color="primary" label outlined>
            {{ item.priority }}
        </v-chip>
        </td>
        </template>

       <template v-slot:[`item.actions`]="{ item }">   
        <v-btn color="blue" @click="detailItem(item)">
           <!-- <v-icon>mdi-heart</v-icon> -->Note
         </v-btn>
         <v-btn color="green" @click="editItem(item)">
           <!-- <v-icon>mdi-account</v-icon> -->Edit
         </v-btn>
         <v-btn color="red" @click="deleteItem(item)">
           <!-- <v-icon>mdi-heart</v-icon> -->Delete
         </v-btn>
       </template>
     </v-data-table>
   </v-card>

   <!-- del dialog -->
    <v-dialog v-model="dialogDelete" max-width="500px">
        <v-card>
            <v-card-title>Delete</v-card-title>
            <v-card-text>Apakah anda yakin menghapus?</v-card-text>
            <v-card-actions>
            <v-btn color="primary" text @click="dialogDelete = false">Close</v-btn>
            <v-btn color="primary" text @click="DeleteSebenarnya">Delete</v-btn>
            </v-card-actions>
        </v-card>
    </v-dialog>

    <!-- detail dialog -->
    <v-dialog v-model="dialogDetail" max-width="500px">
        <v-card>
            <v-card-title>{{detail.task}}</v-card-title>
            <v-container false>
                <v-chip v-if="detail.priority == 'Penting'" color="red" label outlined>
                    {{ detail.priority }}
                </v-chip>
                <v-chip v-else-if="detail.priority == 'Biasa'" color="success" label outlined>
                    {{ detail.priority }}
                </v-chip>
                <v-chip v-else color="primary" label outlined>
                    {{ detail.priority }}
                </v-chip>
            </v-container>
            <v-card-text>{{detail.note}}</v-card-text>
            <v-card-actions>
            <v-btn color="primary" text @click="dialogDetail = false">Close</v-btn>
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
    index:null,
     search: null,
     filter:null,
     dialog: false,
     editedIndex: -1,
     dialogDelete: false,
     dialogDetail: false,
     headers: [
       {
         text: "Task",
         align: "start",
         sortable: true,
         value: "task",
       },
       { text: "Priority", value: "priority" },
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
     detail: {
        task: null,
        priority: null,
        note: null,
     },
   };
 },
 methods: {
    deleteItem(item){
        this.index = this.todos.indexOf(item)
        this.dialogDelete = true
    },
    DeleteSebenarnya(item) {
        this.todos.splice(this.index,1)
        this.dialogDelete = false
    },
    detailItem(item){
        this.detail = {
                task: item.task,
                priority: item.priority,
                note: item.note,
            }
        this.dialogDetail = true
    },
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
