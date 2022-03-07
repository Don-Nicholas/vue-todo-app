<template>
  <b-container class="w-50">
    <h1 id="header" ref="header">Todo List</h1>
        <b-form @submit="onSubmit" @reset="onReset" v-if="show" class="mt-5">
          <b-form-group
            id="input-group-1"
            label="Name:"
            label-for="input-1"
          >
            <b-form-input
              id="input-1"
              v-model="form.name"
              type="text"
              required
              placeholder="Enter Name"
            >
            </b-form-input>
          </b-form-group>
           <b-form-group
            id="input-group-2"
            label="E-mail:"
            label-for="input-2"
          >
            <b-form-input
              id="input-2"
              v-model="form.email"
              type="email"
              required
              placeholder="Enter E-mail"
            >
            </b-form-input>
          </b-form-group>

          <template >
            <b-button
            v-show="form.update == false"
             type="submit" class="my-3 w-"
            variant="primary"
          >Submit</b-button>
          
          </template>
          <b-button
            v-show="form.update" @click="updateUser2" class="m-2"
            variant="success"
          >Done</b-button>
          <b-button
            v-show="form.update" @click="cancelUpdate" class="m-2"
            variant="danger"
          >Cancel</b-button>
         
        </b-form>
    
        <b-table
          striped hover :items="items" :fields="fields" :tbody-tr-class="rowClass"
           class="my-5"
        >
          <template #cell(actions)="row">
            <b-button variant="success" size="sm" @click="updateUser(row.item.id)" class="mr-2">
              Update
            </b-button>
            <b-button :disabled="form.update" variant="danger" size="sm" @click="deleteUser(row.item.id)" class="mr-1">
              Delete
            </b-button>
          </template>
        </b-table>  
      
  </b-container>
</template>

<script>
  export default {
    el:"#header",
    data() {
      return {
        form: {
          email: '',
          name: '',
          update:false
        },
        show: true,
        items: [],
        fields: [
          {key:'name', label:'Name', sortable:true, sortDirection:'desc'},
          {key:'email', label:'E-mail', class:'text-center'},
          {key:'actions', label:'Actions'}
        ],
        con:0
      }
    },
    methods: {
      rowClass(item) {
        if(!item.update) {
          return 'table-light'
        }
        if(!item.update) {
          return 'table-warning'
        }
      },
      onSubmit(event) {
        event.preventDefault()
        this.items.push(
          {
            id:this.con,
            name:this.form.name,
            email:this.form.email,
            update:false
          }
        )
        this.con++
        this.onReset()
      },
      onReset() {
        // evt.preventDefault()
        this.form.email = '',
        this.form.name = '',
        this.show = false,
        this.$nextTick(()=>{
          this.show = true
        })
      },
        deleteUser(id) {
        for(let index = 0; index < this.items.length; index++)
        {
          if(id == this.items[index].id)
          {
            this.items.splice(index, 1);
          }
        }
      },
      updateUser(id) {
      for(let index = 0; index < this.items.length; index++) {
        if(id == this.items[index].id)
        {
          this.form.email = this.items[index].email,
          this.form.name = this.items[index].name,
          this.form.update = true,
          this.items[index].update = true
          this.$refs.header.innerText = "Update Todo List"
        }
      }
    },
    updateUser2() {
      for(let index = 0; index < this.items.length; index++)
      {
        if(this.items[index].update) {
          this.items[index].name = this.form.name,
          this.items[index].email = this.form.email,
          this.items[index].update = false,
          this.form.email = '',
          this.form.name = '',
          this.form.update = false
           this.$refs.header.innerText = "Todo List"
        }
      }
    },
    cancelUpdate() {
      this.form.email = '',
      this.form.name = '',
      this.form.update = false
      for(let index = 0; index < this.items.length; index++)
      {
        this.items[index].update = false
      }
    }
    }
  }
</script>


<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
