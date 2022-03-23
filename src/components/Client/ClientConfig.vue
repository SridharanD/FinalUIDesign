<template>
  <div>
    <div>
      <center>
        <form id="newform">
          <div id="title">Configuration File</div>
          <hr />

          <table>
            <tr>
              <th><label for="clientname">Client Name</label></th>
              <td>
                <div class="selectloc">
                  <md-field>
                    <md-select v-model="clientname" class="select-opt">
                      <md-option
                        v-for="(item, index) in info"
                        :value="item.name"
                        :key="index"
                        >{{ item.name }}</md-option
                      >
                    </md-select>
                  </md-field>
                </div>
              </td>
            </tr>

            <tr>
              <th><label for="LocationName">Location Name</label></th>
              <td>
                <div class="selectloc">
                  <md-field>
                    <md-select v-model="OrgLocation" class="select-opt">
                      <md-option
                        v-for="(items, index) in infodetails"
                        :value="items.locationName"
                        :key="index"
                        >{{ items.locationName }}</md-option
                      >
                    </md-select>
                  </md-field>
                </div>
              </td>
            </tr>
            <tr>
              <th><label for="Device Name">Device Name</label></th>
              <td>
                <div class="selectloc">
                  <md-field>
                    <md-select
                      v-model="devName"
                      id="devicename"
                      class="select-opt"
                    >
                      <md-option
                        v-for="(dataitem, index) in details"
                        :value="dataitem.deviceName"
                        :key="index"
                        >{{ dataitem.deviceName }}
                      </md-option>
                    </md-select>
                  </md-field>
                </div>
              </td>
            </tr>
            <tr>
              <td>
                <button id="button-1" v-on:click="reset()">Cancel</button>
              </td>
              <td>
                &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;
                &nbsp;
                <button id="button-2" v-on:click="Download()">Download</button>
              </td>
            </tr>
          </table>
        </form>
      </center>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "ClientConfig",
  data: function () {
    return {
      clientname: "",
      details: [],
      info: {},
      infodetails: [],
      IsVisible: true,
      OrgLocation: null,
      devName: "",
      data: {},
    };
  },
  created() {
    axios.get("http://localhost:6060/api/Home/getClientName").then((res) => {
      this.info = res.data;
      //console.log(this.info)
    });
  },

  methods: {
    reset: function () {
      document.getElementById("newForm").reset();
    },
    Download: function () {
      this.details = this.details.filter((x) => x.deviceName == this.devName);
      if (this.details.deviceName == null) {
        alert("This Location has No Registered Device");
      }
      //console.log("this is the sample",this.details);

      // var m = module.globeDeployment
      // m = this.details

      var data =
        `module.exports = {
globeIdentification:"` +
        this.details[0].deviceName +
        `",
globeDeployment: "` +
        this.details[0].clientName +
        " " +
        this.details[0].locationName +
        `",
name: "GlobeChek_DESKTOP",
version: "6.0.13",
environment: "dev",
login: "api",
ftpuser: "ftpuser",
ftppsw: "GlobeChekPass1!",
ftphost: "caecvm.eastus2.cloudapp.azure.com",
urlPrereg:
"https://globecheckportal-globecheckapi.azurewebsites.net/api/v1/Desktop` +
        this.details[0].clientID +
        "~" +
        this.details[0].deviceID +
        `/",
urlPortal: "https://gcdevstor.z20.web.core.windows.net/",
urlAPI:"https://globecheckportal-globecheckapi.azurewebsites.net/api/v1/Desktop/` +
        this.details[0].clientID +
        "~" +
        this.details[0].deviceID +
        `/",
urlCompanyWeb: "https://www.globechek.com/",
va: false,
maxHeight: "73",
};`;

      //var all = new datamodel();
      // all.globeIdentification = this.details[0].deviceName;
      // all.globeDeployment = this.details[0].clientName+" "+this.details[0].locationName;
      // all.urlPrereg = "https://globecheckportal-prod-api.azurewebsites.net/api/v1/Desktop/"+this.details[0].clientID+'~'+this.details[0].deviceID+"/";
      // all.urlAPI = "https://globecheckportal-prod-api.azurewebsites.net/api/v1/Desktop/"+this.details[0].clientID+`~`+this.details[0].deviceID+"/";

      var file = new File([data], { type: "" });
      let link = document.createElement("a");
      link.href = window.URL.createObjectURL(file);
      link.download = "active.config.js";
      link.click();
      //console.log("answer",all)
    },
  },

  watch: {
    clientname() {
      axios
        .get("http://localhost:6060/api/Home/getGlobeDetails", {
          params: {
            ClientName: this.clientname,
          },
        })
        .then((res) => {
          this.infodetails = res.data;
        });
      if (this.res == []) {
        alert("This Client has No Registered Location");
      }
    },

    OrgLocation() {
      return (this.details = this.infodetails.filter(
        (x) => x.locationName == this.OrgLocation
      ));
      console.log(details);
    },
  },
};
</script>

<style>
#title {
  font-size: 25px;
  text-align: initial;
  margin-top: 20px;
  margin-left: 20px;
  margin-bottom: 20px;
}

#button-2 {
  align-items: center;
  background-color: #0a66c2;
  border-color: #0a66c2;
  border-radius: 100px;
  box-sizing: border-box;
  color: #ffffff;
  cursor: pointer;
  display: inline-flex;
  font-family: -apple-system, system-ui, system-ui, "Segoe UI", Roboto,
    "Helvetica Neue", "Fira Sans", Ubuntu, Oxygen, "Oxygen Sans", Cantarell,
    "Droid Sans", "Apple Color Emoji", "Segoe UI Emoji", "Segoe UI Symbol",
    "Lucida Grande", Helvetica, Arial, sans-serif;
  font-size: 16px;
  font-weight: 600;
  justify-content: center;
  line-height: 20px;
  max-width: 480px;
  min-height: 40px;
  min-width: 0px;
  overflow: hidden;
  padding: 0px;
  padding-left: 20px;
  padding-right: 20px;
  text-align: center;
  touch-action: manipulation;
  transition: background-color 0.167s cubic-bezier(0.4, 0, 0.2, 1) 0s,
    box-shadow 0.167s cubic-bezier(0.4, 0, 0.2, 1) 0s,
    color 0.167s cubic-bezier(0.4, 0, 0.2, 1) 0s;
  user-select: none;
  -webkit-user-select: none;
  vertical-align: middle;
}
#button-2:hover,
#button-2:focus {
  background-color: #ffffff;
  color: #0a66c2;
}
#button-2:active {
  background: #09223b;
  color: rgb(255, 255, 255, 0.7);
}
#button-2:disabled {
  cursor: not-allowed;
  background: rgb(255, 255, 255);
  color: rgba(255, 254, 254, 0.994);
}

#button-1 {
  align-items: center;
  background-color: #ffffff;
  border-color: #444444;
  border-radius: 100px;
  box-sizing: border-box;
  color: #444444;
  cursor: pointer;
  display: inline-flex;
  font-family: -apple-system, system-ui, system-ui, "Segoe UI", Roboto,
    "Helvetica Neue", "Fira Sans", Ubuntu, Oxygen, "Oxygen Sans", Cantarell,
    "Droid Sans", "Apple Color Emoji", "Segoe UI Emoji", "Segoe UI Symbol",
    "Lucida Grande", Helvetica, Arial, sans-serif;
  font-size: 16px;
  font-weight: 600;
  justify-content: center;
  line-height: 20px;
  max-width: 480px;
  min-height: 40px;
  min-width: 0px;
  overflow: hidden;
  padding: 0px;
  padding-left: 20px;
  padding-right: 20px;
  text-align: center;
  touch-action: manipulation;
  transition: background-color 0.167s cubic-bezier(0.4, 0, 0.2, 1) 0s,
    box-shadow 0.167s cubic-bezier(0.4, 0, 0.2, 1) 0s,
    color 0.167s cubic-bezier(0.4, 0, 0.2, 1) 0s;
  user-select: none;
  -webkit-user-select: none;
  vertical-align: middle;
}
#button-1:hover,
#button-1:focus {
  background-color: #d4d1d1;
}
#button-1:active {
  background: #09223b;
  color: rgb(255, 255, 255, 0.7);
}
#button-1:disabled {
  cursor: not-allowed;
  background: rgb(255, 255, 255);
  color: rgba(255, 254, 254, 0.994);
}

label {
  color: #000000de;
  font-size: 14px;
  padding-bottom: 5px;
  display: block;
  text-align: right;
}

form {
  background: #fff;
  box-shadow: 0 30px 60px 0 rgba(90, 116, 148, 0.4);
  border-radius: 5px;
  max-width: 480px;
  margin-left: auto;
  margin-right: auto;
  padding-top: 5px;
  padding-bottom: 5px;
  left: 0;
  right: 0;
  position: absolute;
}

th {
  text-align: right;
}

div {
  font-family: "Roboto", "Helvetica", "Arial", sans-serif;
}

.select-opt {
  border: 2px solid rgb(76, 140, 235);
  display: flex;
  align-items: center;
  border-radius: 3px;
}
.selectloc {
  margin: 0px 0 0px;
  padding-top: 0px;
  width: 90%;
  position: inherit;
  padding-left: 12px;
}
.selectloc .md-input {
  padding-left: 12px;
}
input#md-input-dnadyiupq {
  padding-left: 12px;
}

/*select {
  height: 20px;
  width: 100px;
  padding: 12px 20px;
  margin: 8px 0;
  display: inline-block;
  border: 3px solid rgb(27, 143, 211);
  border-radius: 4px;
  box-sizing: border-box;
} */
</style>