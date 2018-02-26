<template>
  <div>
    <div v-for="a in classes">
      a
    </div>
  </div>
</template>

<script>
  let config = require("../../config/config");
  const ip = config.ip;
  let moment = require("moment");
  let $ = require("jquery");
  let _ = require("underscore");
    export default {
        name: "update-class",
        data(){
          return {
            classes:[],//所有
          }
        },
      methods:{

      },
      beforeMount:function () {
          let self = this;
          let temp = new Set();


          $.post(`http://${ip}/course/allInfo`,function (response) {
            if(response.code == 1001){
              console.log(response.data)
              console.table(response.data)

              for(let i = 0;i < response.data.length;i++){
                console.log(response.data[i].no)
                temp.add(response.data[i].no);
              }


              for (let j of temp){
                self.classes.push({
                  no:j,
                  stages:[]
                })
              }

              for(let i = 0;i< response.data.length;i++){
                for(let j = 0;j<self.classes.length;j++){
                  if(self.classes[j].no == response.data[i].no){
                    self.classes[j].stages.push(response.data[i]);
                  }
                }
              }
              console.log("classes")
              console.log(self.classes)

            }

          })
        // console.log(self.classes)


      }
    }
</script>

<style scoped>

</style>
