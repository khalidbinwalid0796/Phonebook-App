<template>
  <div>
    <nav class="panel column is-offset-2 is-8">
      <p class="panel-heading">
        Vuejs Phonebook 
        <button class="button is-primary is-outlined" @click="addModal">
          Add New
        </button>
        <span class="is-pulled-right" v-if="loading">
          <i class="fa fa-refresh fa-spin fa-2x fa-fw" ></i>
        </span>
      </p>
      <div class="panel-block">
        <p class="control has-icons-left">
          <input class="input is-small" type="text" placeholder="search" v-model="searchItem">
          <span class="icon is-small is-left">
            <i class="fa fa-search"></i>
          </span>
        </p>
      </div>
     
     
      <a class="panel-block" v-for="item, key in temp">
        <span class="column is-9">
          {{ item.name }}
        </span>
        <span class="panel-icon column is-1">
          <i class="has-text-danger fa fa-trash" aria-hidden="true" @click="delData(key,item.id)"></i>
        </span>
        <span class="panel-icon column is-1">
          <i class="has-text-info fa fa-edit" aria-hidden="true" @click="updateModal(key)"></i>
        </span><span class="panel-icon column is-1">
          <i class="has-text-primary fa fa-eye" aria-hidden="true" @click="showModal(key)"></i>
        </span>
      </a>

    </nav>
    <Add :propdata = 'addActive' @closeRequest='close'></Add>
    <Show :propdata = 'showActive' @closeRequest='close'></Show>
    <Update :propdata = 'updateActive' @closeRequest='close'></Update>
  </div>  
</template>

<script>
  let Add =require('./Add.vue').default;
  let Show =require('./Show.vue').default;
  let Update =require('./Update.vue').default;
export default {
  components:{Add,Show,Update},
  data(){
    return{
      addActive:'',
      showActive:'',
      updateActive:'',
      lists:{},
      errors:{},
      loading:false,
      searchItem:'',
      temp:''
    }
  },

/*
  const res = conArr.filter((currentValue, index, arr) => {
      return arr.indexOf(currentValue) === index; //return unique element of array//true return kore
  });

  //search mechanism for name 
    //static
    searchItem(){
    if(this.searchItem.length > 0){
        this.lists = this.lists.filter((item)=>{
          return item.name.toLowerCase().indexOf(this.searchItem.toLowerCase()) > -1 
        })
    }
  }

  //dynamic
      searchItem(){
      if(this.searchItem.length > 0){
          this.temp = this.lists.filter((item)=>{
            return item.name.toLowerCase().indexOf(this.searchItem.toLowerCase()) > -1 
          })
      }else{
        this.temp = this.lists
      }
    }

*/

  watch:{
      //for all field
      searchItem(){
        if (this.searchItem.length > 0) {
          this.temp = this.lists.filter((item) => {
              console.log(Object.keys(item))
            return Object.keys(item).some((key)=>{
              //console.log(item[key]);
              let string = String(item[key]) //item[key]
              return string.toLowerCase().indexOf(this.searchItem.toLowerCase())>-1
              // console.log(string)
            })
          });
          // console.log(result)
        }else{
          this.temp = this.lists
        }
      }
  },
  mounted(){
    axios.post('/getData')
    .then((response) => {
      this.lists = this.temp = response.data
    })
    .catch((error) => this.errors = error.response.data.errors)
  },
  methods:{
    addModal(){
      this.addActive = 'is-active';
    },
    showModal(key){
      this.$children[1].lists = this.temp[key] //pass data in show component in empty lists object
      this.showActive = 'is-active';
    },
    updateModal(key){
      this.$children[2].lists = this.temp[key] //pass data in update component in empty lists object
      this.updateActive = 'is-active';
    },    
    close(){
      this.addActive = this.showActive = this.updateActive ='';
    },
    delData(key,id){ //for item delete from database id used, from lists key used
      if(confirm("Are you sure ?")){
        axios.delete(`/phonebook/${id}`)
              .then((response)=> {
                this.loading = !this.loading
                this.lists.splice(key,1);
                this.loading = !this.loading
              })
              .catch((error) => this.errors = error.response.data.errors)
      }
    }
  }
}
</script>