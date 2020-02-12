<template>
  <div id="app">
    <div class="container-fluid bg-light">
      <div class="row">
        <div class="col-lg-4 col-md-4 col-sm-10 offset-lg-4 offset-md-4 offset-lg-1" placeholder="Select">
          <div class="d-flex flex-column justify-content-center" style="height: 100vh">
            <div class="card shadow-sm border-0">
              <div class="card-body">
                <edd-select v-bind:options="options"></edd-select>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import EddSelect from './components/EddSelect.vue'

export default {
  name: 'App',
  components: {
    EddSelect
  },
  data(){
    return {
      options: [
        {id: 1, label: "Option 1"},
        {id: 2, label: "Option 2"},
        {id: 3, label: "Option 3"},
        {id: 4, label: "Option 4"},
      ]
    }
  }
}
</script>
<style lang="scss">
  @import "~bootstrap/scss/bootstrap"
</style>
