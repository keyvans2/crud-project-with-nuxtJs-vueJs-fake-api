<template>
  <div>
    <div class="insertParent">
      <i class="fa fa-window-close" aria-hidden="true" @click="closeInsert"></i>
      <div class="insertFormCon">
        <form action="" @submit.prevent="emitData">
          <textarea class="form-control" rows="5" placeholder="Type your todo..." v-model="insertContent" autofocus />
          <button class="btn btn-success mt-2 float-left">Insert</button>
            </form>
        </div>
      </div>
  </div>
</template>

<script>
export default {
    watch: {
        insertContent(){
            if (this.emptyVal) {
                this.insertContent=''
                this.$emit('falseEmptyValue',false)                
            }
        }
    },
    props:['emptyVal'],
    data () {
        return {
            insertContent:'',
        }
    },
    methods: {
        emitData(){
            
        return this.$emit('insertContent',this.insertContent)
        
        },
        closeInsert(){
            this.$emit('closeInsert',false)
        }
    }
}
</script>

<style>
.insertParent{
    width: 100%;
    height: 100%;
    background-color: rgba(223, 235, 233, 0.692);
    position: fixed;
    z-index: 999;
    top: 0;
    left: 0;
}
.insertFormCon{
    width: 30%;
    height: auto;
    /* background-color: dodgerblue; */
    margin: 30px auto;
    border: 1px solid dodgerblue;
    border-radius: 5px 5px 5px 0;
}
</style>
