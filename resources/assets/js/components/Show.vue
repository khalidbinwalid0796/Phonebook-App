<template>
  <div class="modal" :class="propdata">
    <div class="modal-background"></div>
    <div class="modal-card">
    <header class="modal-card-head">
      <p class="modal-card-title">Modal title</p>
      <button class="delete" aria-label="close" @click="closeModal"></button>
    </header>
      <section class="modal-card-body">
        <li class="panel-block">
          <label class="column is-2"><b>Name</b></label> {{ lists.name }}
        </li>

        <li class="panel-block">
          <label class="column is-2"><b>Phone</b></label> {{ lists.phone }}
        </li>

        <li class="panel-block">
          <label class="column is-2"><b>Email</b></label> {{ lists.email }}
        </li>

      </section>
    <footer class="modal-card-foot">
      <button class="button" @click="closeModal">Cancel</button>
    </footer>
  </div>
</div>
</template>
<script>
export default {
  props:['propdata'],
  data(){
    return{
      lists:{},
      errors:{}
    }
  },
  methods:{
    closeModal(){
      this.$emit('closeRequest')
    }
  }
}
</script>