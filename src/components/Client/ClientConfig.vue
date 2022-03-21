<template>
  <div> 
        <div><center>
            <form >
                 <md-card class="md-layout-item md-size-50 md-small-size-100">
        <md-card-header>
          <div class="md-title">Configuration File</div>
        </md-card-header>

        <md-card-content>
           <div >
        <md-field>
          <label for="clientname">Client Name</label>
          <md-select v-model="clientname" name="clientname" id="clientname">
            <md-option v-for="item in info"  :value='item.name' :key='item' >{{item.name}}</md-option>
          </md-select>
        </md-field>
      </div>


    <div >
        <md-field>
          <label for="LocationName">Location Name</label>
          <md-select v-model="OrgLocation" name="clientname" id="clientname">
            <md-option v-for="items in infodetails"  :value='items.locationName' :key='items.deviceID' >{{items.locationName}}</md-option>
          </md-select>
        </md-field>
    </div>

    <div >
        <md-field>
          <label for="Device Name">Device Name</label>
          <md-select v-model="devName" id="devicename">
            <md-option v-for="dataitem in details"  :value='dataitem.deviceName' :key='dataitem.deviceID'  >{{dataitem.deviceName}} </md-option>
          </md-select>
        </md-field>
    </div>
    
    <md-button class="md-raised md-primary" v-on:click="Download()">Download Config File</md-button>


</md-card-content>

</md-card>
</form>
</center>

</div>

</div>

       




  
</template>

<script>
import axios from 'axios'


export default {
    name:"ClientConfig",
data:function() {
    return {

      clientname:'',
      details:[],
      info:{},
      infodetails:{},
      IsVisible:true,
      OrgLocation:null,
      devName:'', 
      data:{}  
    }
}, 
created(){

   axios.get('http://localhost:6060/api/Home/getClientName')
          .then((res)=>{
           this.info=res.data;
           //console.log(this.info)
        }
        )
       
},

 methods : {
   
    Download:function(){
        this.details=this.infodetails.filter(x=> x.deviceName== this.devName);
        //console.log("this is the sample",this.details);
        
        // var m = module.globeDeployment
        // m = this.details

var data = `module.exports = {
globeIdentification:"`+this.details[0].deviceName+`",
globeDeployment: "`+this.details[0].clientName+" "+this.details[0].locationName+`",
name: "GlobeChek_DESKTOP",
version: "6.0.13",
environment: "dev",
login: "api",
ftpuser: "ftpuser",
ftppsw: "GlobeChekPass1!",
ftphost: "caecvm.eastus2.cloudapp.azure.com",
urlPrereg:
"https://globecheckportal-globecheckapi.azurewebsites.net/api/v1/Desktop/api/v1/Desktop/`+this.details[0].clientID+"~"+this.details[0].deviceID +`/",
urlPortal: "https://gcdevstor.z20.web.core.windows.net/",
urlAPI:"https://globecheckportal-globecheckapi.azurewebsites.net/api/v1/Desktop/`+this.details[0].clientID+"~"+this.details[0].deviceID +`/",
urlCompanyWeb: "https://www.globechek.com/",
va: false,
maxHeight: "73",
};`


      //var all = new datamodel();
      // all.globeIdentification = this.details[0].deviceName;
      // all.globeDeployment = this.details[0].clientName+" "+this.details[0].locationName;
      // all.urlPrereg = "https://globecheckportal-prod-api.azurewebsites.net/api/v1/Desktop/"+this.details[0].clientID+'~'+this.details[0].deviceID+"/";
      // all.urlAPI = "https://globecheckportal-prod-api.azurewebsites.net/api/v1/Desktop/"+this.details[0].clientID+`~`+this.details[0].deviceID+"/";

      var file = new File([ data  ], { "type" : "" });
      let link = document.createElement('a')
      link.href = window.URL.createObjectURL(file)
      link.download = 'active.config.js'
      link.click()
      //console.log("answer",all)

    },

},

watch:{
      
   clientname(){
        axios.get('http://localhost:6060/api/Home/getGlobeDetails',{
            params:{
                ClientName: this.clientname,
            }
        }).then((res)=>{
           this.infodetails=res.data;})
           },

    OrgLocation(){
        return this.details=this.infodetails;
    },

},  


}



</script>

<style>


</style>