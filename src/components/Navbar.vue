<template>
  <nav class="navbar navbar-light bg-light">
    <a class="navbar-brand" style="margin-top: -32px;" href="#">
      <img src="../assets/eczane.png" width="60" height="60" class="d-inline-block align-top" alt="">
      Nöbetçi Eczane App
    </a>

    <div class="row me-0">
      <div class="input-field col s3">
        <input v-model="searchcity" type="text" class="validate " placeholder="Şehir Girin">
        <label class="active" >Şehir</label>
      </div>
      <div class="input-field col s3">
        <input v-model="searchdistrict" type="text" class="validate" placeholder="İlçe Girin">
        <label class="active" >İlçe</label>
      </div>
      <div class="input-field col s6">
        <a @click="emitNavbar" class="waves-effect waves-light btn-small">Bul</a>
      </div>
    </div>
  </nav>
</template>
<script>
  export default{
    data () {
      return {
        searchcity:null,
        searchdistrict:null
      }
    },
    methods:{
      emitNavbar(){
        this.$emit("navbarData",{
          searchcity:this.searchcity,
          searchdistrict:this.searchdistrict
        })
      }
    }
  }
</script>