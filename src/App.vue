<template>

  <div id="app">

    <Navbar @navbarData="navbarDataOperation"/>


    <!--<div class="d-flex flex-column align-items-stretch flex-shrink-0 bg-white w-25 divscrool">
      <div class="list-group list-group-flush border-bottom scrollarea">
        <a href="#" class="list-group-item list-group-item-action active py-3 lh-tight" aria-current="true">
          <div class="d-flex w-100 align-items-center justify-content-between">
            <strong class="mb-1">List group item heading</strong>
            <small>Wed</small>
          </div>
          <div class="col-10 mb-1 small">Some placeholder content in a paragraph below the heading and date.</div>
        </a>
        <a href="#" class="list-group-item list-group-item-action py-3 lh-tight">
          <div class="d-flex w-100 align-items-center justify-content-between">
            <strong class="mb-1">List group item heading</strong>
            <small class="text-muted">Tues</small>
          </div>
          <div class="col-10 mb-1 small">Some placeholder content in a paragraph below the heading and date.</div>
        </a>
        <a href="#" class="list-group-item list-group-item-action py-3 lh-tight">
          <div class="d-flex w-100 align-items-center justify-content-between">
            <strong class="mb-1">List group item heading</strong>
            <small class="text-muted">Mon</small>
          </div>
          <div class="col-10 mb-1 small">Some placeholder content in a paragraph below the heading and date.</div>
        </a>
      </div>
    </div>-->

    <div v-if="pharmacylist.length<=0" class="alert alert-warning text-center mt-5 w-50 me-auto ms-auto mt-5">
      Lütfen şehir ismi girin !!!
    </div>

    <div v-if="pharmacylist.length>0" class="d-flex flex-column align-items-stretch flex-shrink-0 bg-white w-25 divscrool">
      <div class="list-group list-group-flush border-bottom scrollarea">
        <a @click="clickedPharmacy=index" v-for="(list,index) in pharmacylist" href="#" class="list-group-item list-group-item-action py-3 lh-tight" :class="clickedPharmacy==index ? 'active' : '' " aria-current="true">
          <div class="d-flex w-100 align-items-center justify-content-between">
            <strong class="mb-1">{{list.name}}</strong>
            <small>{{list.dist}}</small>
          </div>
          <div class="col-10 mb-1 small">{{list.address}}</div>
        </a>
      </div>
    </div>


    <!-- konum bilgisi isme göre yapıldı, ismi aynı olan eczanelerde farklı konumlar gösterilebilir, bunun için konum bilgisi address'e göre tanımlanırsa daha az hata oluşur -->
    <div v-if="clickedPharmacy!=null" class="w-75 rightSide">
      <div class="card col-12">
        <div class="card-header">
          <h5 style="color:#D61607;" class="card-title">{{pharmacylist[clickedPharmacy].name}}</h5>
        </div>
        <div class="card-body">
          <p class="card-text"><i class="material-icons">location_on</i> {{pharmacylist[clickedPharmacy].address}} / {{pharmacylist[clickedPharmacy].dist}}</p>
          <p class="card-text"><i class="material-icons">phone</i> {{pharmacylist[clickedPharmacy].phone}}</p>
          <iframe width="100%" height="380" :src="'https://maps.google.com/maps?width=380&amp;height=380&amp;hl=en&amp;q='+pharmacylist[clickedPharmacy].name+'+(Title)&amp;ie=UTF8&amp;t=&amp;z=19&amp;iwloc=B&amp;output=embed'" frameborder="0" scrolling="no" marginheight="0" marginwidth="0"></iframe>
        </div>
      </div>
    </div>


  </div>
</template>

<script>
  import axios from "axios"
  import Navbar from "./components/Navbar"

  export default {
    components:{
      Navbar
    },
    data () {
      return {
        searchcity:null,
        searchdistrict:null,
        apikey:"collectapi apikey gir",
        pharmacylist:[],
        clickedPharmacy:null
      }
    },
    methods:{
      findPharmacy(){
        //türkiye 

        /*axios.get(`https://api.collectapi.com/health/dutyPharmacy?ilce=`+this.searchdistrict+`&il=`+this.searchcity,{
          headers:{
            "content-type": "application/json",
            "authorization": this.apikey
          }
        }).then((res)=>{
          console.log(res.data.result)
          this.pharmacylist = res.data.result
        })*/

        //almanya

        axios.get(`https://api.collectapi.com/germanyPharmacy/dutyPharmacy?city=`+this.searchcity,{
          headers:{
            "content-type": "application/json",
            "authorization": this.apikey
          }
        }).then((res)=>{
          console.log(res.data.result)
          this.pharmacylist = res.data.result
        }).catch(err=>{
          console.log(err)
        })
      },
      navbarDataOperation({searchcity,searchdistrict}){
        this.searchcity=searchcity
        this.searchdistrict=searchdistrict
        this.findPharmacy()
      }
    }
  }
</script>

<style>
 @import url('https://fonts.googleapis.com/css2?family=Roboto&display=swap');
 nav{
  height: 10vh;
}
#app{
  height: 100vh;
  overflow: hidden;
  position: relative;
  font-family: 'Roboto', sans-serif;

}
.divscrool{
  height: 93vh;
  overflow: scroll;
}
::-webkit-scrollbar {
  width: 5px;
}
::-webkit-scrollbar-thumb {
  background: #D61607;
}
.rightSide{
  position: absolute;
  top: 57px;
  right: 0;
}
</style>
