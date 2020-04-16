<template>
  <div class="hello"> 
   <el-row>
    <el-col :span="24"><input class="input-file" type="file" @change="exportData"
            accept=".csv, application/vnd.openxmlformats-officedocument.spreadsheetml.sheet, application/vnd.ms-excel" />
      <el-button type="success" size="small" @click="btnClick">{{title}}</el-button></el-col>
   </el-row>

   <el-table
    :data="tableData"
    style="width: 100%;margin-top:30px;" >
    <!-- <el-table-column
      type="index"
      :index="indexMethod">
    </el-table-column> -->
    
    <el-table-column
      prop="__EMPTY"
      >
    </el-table-column>
    <el-table-column
      prop="__EMPTY_1"
     >
    </el-table-column>
    <el-table-column
      prop="__EMPTY_2"
      >
    </el-table-column>
     <el-table-column
      prop="__EMPTY_3"
      >
    </el-table-column>
      <el-table-column
      prop="__EMPTY_4"
      >
    </el-table-column>
      <el-table-column
      prop="__EMPTY_5"
     >
    </el-table-column>
  </el-table>
  <!-- <div v-html="htmlS"></div> -->
 
  </div>
</template>

<script>
import XLSX from 'xlsx'
export default {
  name: 'HelloWorld',
  components: {
     
    },

  props: {
      // type: String,
      // default: "选择excel文件"
  },
   data() {
      return {
        currentDate: new Date(),
        activeName:"first",
        activeIndex: '1',
        activeIndex2: '1',
        title:'导入数据',
        e:null,
        tableData:[],
        htmlS:null
       
       
      };
    },
    methods: {
       btnClick() {
        document.querySelector(".input-file").click();
      },
      exportData(event) {
        if(!event.currentTarget.files.length) {
          return;
        }
        const that = this;
        // 拿取文件对象
        let f = event.currentTarget.files[0];
        //这里已经拿到了excel的file文件，若是项目需求，可直接$emit丢出文件
        that.$emit('getMyExcelData',f);
        // 用FileReader来读取
        let reader = new FileReader();
        // 重写FileReader上的readAsBinaryString方法
        FileReader.prototype.readAsBinaryString = function(f) {
          let binary = "";
          let wb; // 读取完成的数据
          let outdata; // 你需要的数据
          let reader = new FileReader();
          reader.onload = function(e) {
            console.log(e)
            // 读取成Uint8Array，再转换为Unicode编码（Unicode占两个字节）
            let bytes = new Uint8Array(reader.result);
            let length = bytes.byteLength;
            for (let i = 0; i < length; i++) {
              binary += String.fromCharCode(bytes[i]);
            }
            // 接下来就是xlsx
            wb = XLSX.read(binary, {
              type: "binary"
            });
            outdata = XLSX.utils.sheet_to_json(wb.Sheets[wb.SheetNames[0]]);
            //此处可对数据进行处理
                // let arr = [];
                outdata.map(v => {
                  console.log(v)
                    // let obj = {}
                    // obj.code = v['Code']
                    // obj.name = v['Name']
                    // obj.pro = v['province']
                    // obj.cit = v['city']
                    // obj.dis = v['district']
                    // arr.push(obj)
                });
                // _this.da=arr;
                // _this.dalen=arr.length;
                // return arr;
            that.tableData = outdata
            console.log(that.tableData)
            // 导出格式为json，{表头：[]，表数据：[]}
            that.$emit("getResult", outdata);
            
          };
          reader.readAsArrayBuffer(f);
        };
        reader.readAsBinaryString(f);
      },
    },
    created(){
      console.log(XLSX)
    },
    mounted(){}
}
</script>
<style  scoped>

/* 焦点·参考swiper.min.css源文件修改 */
  .item >img{
    width: 500px;
  }
.titleHeader{
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    
}
.titleHeader > span{

 text-align: center; 
 line-height: 50px; 
}
.time {
    font-size: 16px;
    color: #999;
  }
  
  .bottom {
    margin-top: 13px;
    line-height: 12px;
  }

  .button {
    padding: 0;
    float: right;
  }

  .image {
   border-radius: 50px;
    width: 57px;
    display: block;
  }

  .clearfix:before,
  .clearfix:after {
      display: table;
      content: "";
  }
  
  .clearfix:after {
      clear: both
  }
.bg-purple-dark >img{
  
}
 .el-row {
    margin-bottom: 20px;
    &:last-child {
      margin-bottom: 0;
    }
  }
  .el-col {
    border-radius: 4px;
  }
  .bg-purple-dark {
    background: #99a9bf;
  }
  .bg-purple {
    background: #d3dce6;
  }
  .bg-purple-light {
    background: #e5e9f2;
  }
  .grid-content {
    border-radius: 4px;
    min-height: 36px;
  }
  .row-bg {
    padding: 10px 0;
    background-color: #f9fafc;
  }
.imgBox >img{
  width: 100%;
  height: 100%;
}

  .el-carousel__item h3 {
    color: #475669;
    font-size: 14px;
    opacity: 0.75;
    line-height: 200px;
    margin: 0;
  }
  
  .el-carousel__item:nth-child(2n) {
    background-color: #99a9bf;
  }
  
  .el-carousel__item:nth-child(2n+1) {
    background-color: #d3dce6;
  }
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
.input-file {
    display: none;
  }
</style>
