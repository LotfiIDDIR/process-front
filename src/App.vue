<template>
  <div id="app">
    <div class="top">
      <b-container>
        <b-row>
          <b-col class="top-title">Entrainement</b-col>
        </b-row>
     </b-container>
    </div>
    
    <div class="left">
      <b-container>
        <b-row>
          <b-col class="top-subtitle">produit fraudé </b-col>
          <b-col class="top-subtitle">  <select v-model="prod">
              <option>PCS</option>
              <option>MG</option>
              </select>
          </b-col>
        </b-row>
        <b-row>
          <b-col class="top-subtitle">algorithme</b-col>
          <b-col class="top-subtitle">   <select v-model="algo">
              <option>RF</option>
              <option>DT</option>
              </select>
              </b-col>
        </b-row>
        <b-row v-if="prod=='PCS'">
          <b-col class="top-subtitle">intervalle de regroupement</b-col>
          <b-col class="top-subtitle">  <input v-model="inteval" placeholder="minutes"></b-col>
        </b-row>
        <b-row v-if="algo=='RF'">
          <b-col class="top-subtitle">nombre d'arbre </b-col>
          <b-col class="top-subtitle">  <input v-model="numTree" placeholder="nb"></b-col>
        </b-row>
      </b-container>
    </div>
    <div class="right">
      <b-container>
         <b-row>
          <b-col class="top-subtitle">profondeur </b-col>
          <b-col class="top-subtitle">  <input v-model="nb" placeholder="nb"></b-col>
        </b-row>
        <b-row>
          <b-col class="top-subtitle">portion de fraude </b-col>
          <b-col class="top-subtitle">  <input v-model="fraudPortion" placeholder="portion"></b-col>
        </b-row>
        <b-row>
          <b-col class="top-subtitle">portion d'entrainement</b-col>
          <b-col class="top-subtitle">  <input v-model="trainingPortion" placeholder="portion"></b-col>
        </b-row>
        <b-row>
          <b-col class="top-subtitle">fichier</b-col>
          <b-col class="top-subtitle">  <input type="file" @change="handleFileChange"></b-col>
        </b-row>
        <b-row>
          <b-col class="top-subtitle">  <button @click="onUpload">Upload</button></b-col>
        </b-row>
      </b-container>
    </div>
    <div class="bottom">
      <b-row>
          <b-col class="pie"><pie-chart
            v-bind:ratio="recall"
            :stroke-width=1
            label="Rappel"
            v-bind:label-small="recallP"
            color=#40a070
            :opacity=0.7
          /> </b-col>
          <b-col class="pie">  <pie-chart v-bind:ratio="precision"
            :stroke-width=1
            label="Precision"
            v-bind:label-small="precisionP"
            color=#40a070
            :opacity=0.7
          /> </b-col>
          <b-col class="pie"><pie-chart v-bind:ratio="error"
            :stroke-width=1
            label="Erreur"
            v-bind:label-small="errorP"
            color=#40a070
            :opacity=0.7
          /> </b-col>
        </b-row>
    </div>
  </div>
</template>

<script>
import PieChart from './compounents/PieChart'
import axios from 'axios'

export default {
  name: 'app',
  data () {
    return {
      nb:'',
      selectedFile: null,
      algo:'RF',
      prod:'PCS',
      recall:0.99,
      recallP:'',
      trainingPortion:'',
      fraudPortion:'',
      precision:0.97,
      precisionP:'',
      error:0.03,
      errorP:'',
      numTree:'',
      interval:''
    }
  },
  components: {
    PieChart
  },
   methods: {
    handleFileChange(e) {
      console.log(e);
      this.selectedFile = e.target.files[0];
      this.errorP = this.error*100 +'%';
    },
    onUpload(e){
      const fd = new FormData();
      console.log(this.nb);
      fd.append('file',this.selectedFile,this.selectedFile.name);
      axios.post('http://127.0.0.1:9991/train/upload',fd)
        .then(res=>{
          console.log(res)
        })
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
.top-title {
  font-size: 2rem;
}
.top-subtitle {
  font-size: 1.5rem;
  margin-bottom: 1%;
  
}
.top {
  display: flex;
  align-items: center;
  position: relative;
  z-index: 10;
  height: 100px;
  margin-top: 1%;
}
.left {
  display: flex;
  align-items: left;
  text-align: left;
  float: left;
  position: relative;
  z-index: 10;
  height: auto;
  width : 50%;
  margin-top: 1%;
}
.right {
  display: flex;
  align-items: right;
  text-align: right;
  position: relative;
  z-index: 10;
  height: auto;
  width : 50%;
  margin-left: 50%;
  margin-top: 1%;
}
.bottom{
  height: auto;
  width : auto;
  margin-left: 1%;
  margin-top: 1%;
  margin-right: 1%;
}
.pie {
  position: relative;
  height: 10%;
  width : 10%;
  margin-left: 10%;
  margin-right: 10%;
  margin-top: 2%;
}
</style>