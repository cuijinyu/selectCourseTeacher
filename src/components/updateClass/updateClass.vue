<template>
  <div>
    <el-table
      :data="classes"
      style="width: 100%">
      <el-table-column type="expand">
        <template slot-scope="props">
          <el-form label-position="left" inline class="demo-table-expand">
            <!--<el-form-item label="所属店铺">-->
              <!--<span>{{ props.row.shop }}</span>-->
            <!--</el-form-item>-->
            <!--<el-form-item label="商品 ID">-->
              <!--<span>{{ props.row.id }}</span>-->
            <!--</el-form-item>-->
            <!--<el-form-item label="店铺 ID">-->
              <!--<span>{{ props.row.shopId }}</span>-->
            <!--</el-form-item>-->
            <!--<el-form-item label="商品分类">-->
              <!--<span>{{ props.row.category }}</span>-->
            <!--</el-form-item>-->
            <!--<el-form-item label="店铺地址">-->
              <!--<span>{{ props.row.address }}</span>-->
            <!--</el-form-item>-->
            <!--<el-form-item label="商品描述">-->
              <!--<span>{{ props.row.desc }}</span>-->
            <!--</el-form-item>-->
            <div v-for="i in props.row.stages" class="stage">
              <el-form-item label="qishu">
                <span>{{ i.stage }}</span>
              </el-form-item>
              <el-form-item label="classroom">
                <span>{{ i.classroom }}</span>
              </el-form-item>
              <el-form-item label="teacher">
                <span>{{ i.teacher }}</span>
              </el-form-item>
              <el-form-item label="kaikeshijian">
                <span>{{ i.startDateTime }}</span>
              </el-form-item>
              <el-form-item label="jiekeshijian">
                <span>{{ i.endDateTime }}</span>
              </el-form-item>
            </div>
          </el-form>
        </template>
      </el-table-column>
      <el-table-column
        label="编号"
        prop="no">
      </el-table-column>
      <el-table-column
        label="kechengming"
        prop="name">
      </el-table-column>
      <el-table-column
        label="xueshi"
        prop="time">
      </el-table-column>
    </el-table>
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

              for(let i = 0;i < self.classes.length;i++){
                self.classes[i].name = self.classes[i].stages[0].name;
                self.classes[i].time = self.classes[i].stages[0].time;
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
  .stage{
    padding-top: 20px;
    border-bottom: 1px solid rgba(0,0,0,0.2);
  }
  .stage:last-child{
    padding-top: 20px;
    border-bottom: 0px;
  }
</style>
