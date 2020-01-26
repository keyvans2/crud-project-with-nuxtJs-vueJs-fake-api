<template>
  <div>
    <client-only>
      <div class="container">
        <insert-cm v-if="insertStatus" @closeInsert="insertStatus=!insertStatus" @insertContent="insert" :emptyVal="emptyVal" @falseEmptyValue="emptyVal=false"/>
        <div class="subPop" v-if="popUp">
          <i class="fa fa-window-close" aria-hidden="true" @click="popUp=!popUp"></i>
          <div class="popUp">
            <form action="#" @submit.prevent="update(todosData.id)">
              <textarea class="form-control" rows="5" v-model="todosData.editText">

              </textarea>
              <button class="btn btn-block btn-success mt-2">
                Edit
              </button>
            </form>
          </div>
        </div>
        <div class="row">
          <div class="col-lg-2 mt-4">
            <button class="btn btn-success float-left" @click="insertStatus=!insertStatus">
              Add Todo <i class="fa fa-plus" aria-hidden="true"></i>
            </button>
          </div>
          <div class="col-lg-9 mt-4">
            <table class="table-bordered">
              <tr v-for="todo in todos" :key="todo.id">

                <td class="pb-2" style="width:600px;">{{todo.title}}</td>
                <td>

                  <i class="fa fa-trash text-danger text-left" aria-hidden="true" @click="del(todo.id)"></i>
                  <i class="fas fa-edit text-right pl-1 text-primary" @click="edit(todo.id)"></i>
                </td>
              </tr>
            </table>
          </div>
        </div>
      </div>
    </client-only>

  </div>
</template>

<script>
  import insertCm from "../components/insertCm";
  export default {
    created() {
      return this.$axios.$get('https://jsonplaceholder.typicode.com/todos').then((res) => {
        this.todos = res
      })
    },
    components: {
      insertCm,
    },
    data() {
      return {
        popUp: false,
        emptyVal:false,
        todos: '',
        insertStatus: false,
        todosData: {
          id: '',
          editText: ''
        },
      }
    },
    methods: {
      edit(id) {
        this.popUp = true
        this.$axios.$get(`https://jsonplaceholder.typicode.com/todos/${id}`).then((res) => {

          this.todosData.editText = res.title
          this.todosData.id = res.id

        })

      },
      update(id) {
        return this.$axios.$patch(`https://jsonplaceholder.typicode.com/todos/${id}`, {
          title: this.todosData.editText
        }).then((res) => {
         this.popUp = false
         alert(`title changed to :${res.title}`);
        })


      },
      del(id) {
        let conf = confirm('Are You Sure?');
        if (conf) {
          return this.$axios.$delete(`https://jsonplaceholder.typicode.com/todos/${id}`).then((res) => {
            alert('deleted successfully')
          })
        }
      },
      insert(val) {
        return this.$axios.$post(`https://jsonplaceholder.typicode.com/todos`, {
            title: val
          })
          .then((res) => {
            alert(`title:${res.title} added -  id:${res.id}`)
            this.emptyVal=true
           
          })
      }
    }
  }

</script>

<style>
  td {
    border: none !important;
  }

  * {
    text-align: center;
  }

  .subPop {
    width: 100%;
    height: 100%;
    background-color: rgba(223, 235, 233, 0.692);
    position: fixed;
    /* top:40px; */
    z-index: 999;
    left: 0;
    right: 0;
  }

  .popUp {
    width: 30%;
    height: auto;
    /* background-color: dodgerblue; */

    margin: 50px auto;
  }

  .popUp textarea {
    border: 1px solid black;
    /* background-color: rgba(0, 255, 255, 0.486)!important; */
    color: black;
    font-weight: bold;
  }

</style>
