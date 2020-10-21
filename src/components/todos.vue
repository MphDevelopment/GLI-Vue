<template lang="html">

  <section class="todos">
    <h1>TODOS:</h1>
    <input type="text" v-model="message" placeholder="Nom tâche" v-on:change="add()">
    <!--<button v-on:click="add()">Ajouter {{message}} </button> -->
    <br>
    Filter by: 
    <input type="radio" value="all" v-model="filter" name="filter" checked> All 
    <input type="radio" value="todo" v-model="filter" name="filter"> Todo
    <input type="radio" value="doing" v-model="filter" name="filter"> Doing
    <input type="radio" value="done" v-model="filter" name="filter"> Done
    <input type="checkbox" @click="checkAll()" v-model="isCheckAll"> Check ALL
    <ul>
      <li v-for="(item, index) in filterTasks()" :key="item.message">
        <input type="checkbox" v-model="selectedIds" :value="index" @change='updateCheckall()'> 
        {{ item.message }} [{{ item.category }}]
        <button v-on:click="remove(index)">Remove</button>
        <button v-on:click="item.upCategory()">up</button>
        <input v-if="selectedIds.length == 1 && selectedIds.includes(index)" v-on:change="setName(item)" v-model="newname">
      </li>
    </ul>
  

    <footer> # of tasks : {{tasks.length}} filter={{filter}}</footer>
  </section>

  

</template>

<script lang="ts">

  import { Component, Vue } from 'vue-property-decorator';

class Task{
    category: string;

    constructor(private message: string)
    {
      this.category = "todo";
    }

    upCategory(){
      switch(this.category){
        case "todo":
          this.category = "doing";
          break;
        case "doing":
          this.category = "done";
          break;
      }
    }

    setName(st: string){
      this.message = st;
    }


  }

@Component
export default class Todos extends Vue {

  name = 'todos';
  props = [];

  message = '';
  filter = 'all';
  tasks = [];
  newname='';
  selectedIds=[];
  isCheckAll=false;

  checkAll(){
    this.isCheckAll = !this.isCheckAll;
    this.selectedIds = [];

    if (this.isCheckAll){
      this.tasks.forEach( (t, i) => this.selectedIds.push(i));
    }
  }

  updateCheckall(){
    if (this.tasks.length === this.selectedIds.length)
    {
      this.isCheckAll = true;
    } else
    {
      this.isCheckAll = false;
    }
  }

  add() {
    console.log(this.message);
    this.tasks.push(new Task(this.message));
    this.message = '';
  }

  remove(index) {
    console.log("deleting " + this.tasks[index] + " task!");
    this.tasks.splice(index, 1);
  }

  filterTasks(){
    switch(this.filter){
      case "all":
        return this.tasks;
      default:
        console.log(this.filter);
        return this.tasks.filter(t => t.category == this.filter);
    }
  }

  debug(){
    console.log(this.selectedIds);
  }

  setName(task: Task){
    task.setName(this.newname);
    this.newname='';
    }
  
}

</script>

<style scoped lang="scss">
  .todos {

  }
</style>
