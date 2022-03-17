<template>
  <div class="app-container">
    <h2 class="titles">商品配送查询</h2>
    <p class="info"> 受数据传输影响，个别客户的销售数据可能出现微小差异，结算以财务数据为准。</p>
    <p class="info"> 本系统数据自2015年5月10日起进入开发测试，前期数据无法读出，请注意您查询时间段的选取。</p>
    <commonForm :inline="true" :data="getInfoData" :formConfig="formConfig">
    </commonForm>
    <JsonExcel
     v-if="total > 0"
      class = "export-excel-wrapper"
      :data = "downloaddata"
      :fields = "json_fields"
      name = "商品配送单.xls">
      <!-- 上面可以自定义自己的样式，还可以引用其他组件button -->
      <el-button type="primary" size="small">导出EXCEL</el-button>
    </JsonExcel>
    <!-- <el-upload action="" :auto-upload="false" :on-change="upload" :limit="1">
      <el-button size="mini" type="success">上传文件</el-button>
    </el-upload> -->
    <el-table
      :data="tabData"
      element-loading-text="Loading"
      border
      fit
      highlight-current-row
      class="forms"
    >
      <el-table-column
        prop="序号"
        width="120"
        label="序号"
      ></el-table-column>
      <el-table-column
        prop="查询起始日期"
        min-width="120"
        label="查询起始日期"
      ></el-table-column>
      <el-table-column
        prop="查询结束日期"
        width="120"
        label="查询结束日期"
      ></el-table-column>
      <el-table-column
        min-width="130"
        prop="公司编号"
        label="公司编号"
      >
      </el-table-column>
      <el-table-column
        prop="公司名称"
        width="120"
        label="公司名称"
      ></el-table-column>
      <el-table-column prop="实际区域" min-width="130" label="实际区域"></el-table-column>
      <el-table-column label="门店编号" min-width="130" prop="门店编号"></el-table-column>
      <el-table-column label="门店名称" width="120" prop="门店名称">
      </el-table-column>
      <el-table-column label="品名" width="120" prop="品名">
       
      </el-table-column>
      <el-table-column label="规格" width="120" prop="规格"></el-table-column> 
      <el-table-column label="生产单位" width="120" prop="生产单位"></el-table-column> 
      <el-table-column label="数量" width="120" prop="数量"></el-table-column> 
       
    </el-table>
    <!--分页器-->
    <pagination
      v-show="total > 0"
      :total="total"
      :page.sync="getInfoData.offset"
      :limit.sync="getInfoData.limit"
      @pagination="gitList"
    />
  </div>
</template>

<script>
  import * as XLSX from 'xlsx/xlsx.mjs'
  import commonForm from "@/components/CommonForm";
  import { saveAs } from 'file-saver';
  import pagination from "@/components/Pagination";
  import JsonExcel from 'vue-json-excel'
  export default {
    components:{
      commonForm,
      pagination,
      JsonExcel
    },
    data() {
      return {
        json_fields: {
          "序号": "序号",    //常规字段
          "查询起始日期": "查询起始日期", //支持嵌套属性
          "查询结束日期": "查询结束日期",    //常规字段
          "公司编号": "公司编号",    //常规字段
          "公司名称": "公司名称",    //常规字段
          "实际区域": "实际区域",    //常规字段
          "门店编号": "门店编号",    //常规字段
          "门店名称": "门店名称",    //常规字段
          "品名": "品名",    //常规字段
          "规格": "规格",    //常规字段
          "生产单位":"生产单位",
          "数量":"数量"
        },
        tabData:[],
        alldata:[],
        downloaddata:[],
        total:0,
        getInfoData: {
          limit: 10,
          offset: 1,
          startDelTime: [],
          startStartTime:'',
          endStartTime:'',
        },
        formConfig: {
          formItemList: [
            
            {
              label: "",
              value: "startDelTime",
              clearable: true,
              startPlaceholder: "开始日期",
              endPlaceholder: "结束日期",
              type: "daterange",
              dateFormate: "yyyy-MM-dd",
            }
          ],
          operate: [
            {
              name: "搜索",
              value: "primary",
              type: "button",
              handleClick: this.gitList,
              authBtn: true,
            },
            {
              name: "清空搜索条件",
              value: "",
              type: "button",
              handleClick: this.clearInput,
              authBtn: true,
            },
          ],
        },
      }
    },
    mounted(){
      this.readWorkbookFromRemoteFile('https://image.ructrip.com/商品配送单.xlsx');
    },
    methods: {
      clearInput(){
        this.getInfoData = {
          limit: 10,
          offset: 1,
          startDelTime: [],
          startStartTime:'',
          endStartTime:'',
        }
      },
      readWorkbookFromRemoteFile: function (url) {
        
        var xhr = new XMLHttpRequest()
        xhr.open('get', url, true)
        xhr.responseType = 'arraybuffer'
        let _this = this
        xhr.onload = (e) => {
          if (xhr.status === 200) {
            var data = new Uint8Array(xhr.response)
            
            const workbook = XLSX.read(data, {
              type: 'array'
            })
            const wsname = workbook.SheetNames[0]// 取第一张表
            this.alldata= XLSX.utils.sheet_to_json(workbook.Sheets[wsname])// 生成json表格内容
            if(this.alldata && this.alldata.length > 0){
              for(let i = 0;i<this.alldata.length;i++){
                this.alldata[i]['查询起始日期']= this.formatDate(this.alldata[i]['查询起始日期'],'-')
              }
            }
            
            console.log(this.alldata);
          }
          fileReader.readAsBinaryString(files[0])
        }
        xhr.send()
      },
      gitList(){
        if(this.getInfoData.startDelTime && this.getInfoData.startDelTime.length > 0){
          this.getInfoData.startStartTime = this.getInfoData.startDelTime ? this.getInfoData.startDelTime[0] : "";
          this.getInfoData.endStartTime = this.getInfoData.startDelTime ? this.getInfoData.startDelTime[1] : "";
          let searchList = [];
          // debugger
        for(let i = 0;i<this.alldata.length;i++){
          if(this.getInfoData.startStartTime<=this.alldata[i]['查询起始日期'] && this.alldata[i]['查询起始日期'] < this.getInfoData.endStartTime){
            searchList.push(this.alldata[i])
          }
        }
        this.downloaddata = searchList;
        this.total = searchList.length
        this.tabData = searchList.slice((this.getInfoData.offset-1)*this.getInfoData.limit,this.getInfoData.offset*this.getInfoData.limit)
        }else{
          this.downloaddata = [];
          this.total = 0
          this.tabData = []
        }
        
      },
      formatDate(numb, format) {
        const old = numb - 1;
        const t = Math.round((old - Math.floor(old)) * 24 * 60 * 60);
        const time = new Date(1900, 0, old, 0, 0, t)
        const year = time.getFullYear() ;
        const month = time.getMonth() + 1 ;
        const date = time.getDate() ;
        return year + format + (month < 10 ? '0' + month : month) + format + (date < 10 ? '0' + date : date)
      },
    }
  }
</script>
<style scoped>
.export-excel-wrapper{
  margin-bottom: 10px;
  
}
.titles{
  font-size: 20px;
  font-weight: bold;
  color: #666;
  margin-top: 10px;
}
.info{
  margin:10px 0;
  color: #696969;
  font-size: 14px;
}
</style>
