<template>
    <div class="myform">
        <!-- <h2>测测测测测</h2> -->
    <el-table  style="width:100%;overflow-x: auto;overflow-y: auto;"
              :data="transform"
            
              v-loading="loadFlag"
              :element-loading-text="page_data.loadTxt"
              empty-text="没有数据~"
              stripe
              border>

          <el-table-column prop="id"  label="ID" align="center">
      </el-table-column>

       <el-table-column prop="title"  label="名称" align="center">
      </el-table-column>

      <el-table-column prop="desc"  label="内容" align="center">
      </el-table-column>

        <!-- <el-table-column prop="meta_items.course_highlights[0].desc"  label="内容" align="center">
      </el-table-column> -->
      
  <el-table-column   label="图片" align="center">
               <template slot-scope="scope">
                        <el-image
                            class="table-td-thumb"
                            :src="scope.row.list_thumb_url"
                            :preview-src-list="[scope.row.list_thumb_url]"
                        ></el-image>
               </template>              
      </el-table-column>


       <el-table-column prop="created_at" sortable="custom" label="时间" align="center">
      </el-table-column>


    </el-table>

    <el-pagination
  background
  layout="prev, pager, next"
   @current-change="handleCurrentChange"

   :page-size="pagesize"         
   :total="total"
  >
</el-pagination>



    </div>
</template>

<script>
export default {
    name:"myform",
    data(){
        return{
          page_data: {
              loadTxt: '请求列表中',
            },
          loadFlag: false, // 加载flag
          transform:[],
           total:1, //初始页
           pagesize:1,    //    每页的数据
        }

    },
   beforeMount() {
    this.getAdded();
   },

    methods:{
        getAdded(){
        this.$axios.get(`https://wangxiao.uoyc.com/api/courses-test`,{
            headers: {
              // token: sessionStorage.getItem('token')
              "Content-Type": "application/json"
            },
            params: {
              page: this.page,
              size: 10,
            }
             }).then(res => {
               console.log(res);
               this.transform=res.data.data
              //  console.log(this.transform)
                  this.total = res.data.last_page

           });

        },
              //分页点击
           handleCurrentChange(val){
                this.page=val;
                 this.getAdded()
           }   
    }
    

    
}
</script>
<style scoped>
.table-td-thumb {
    display: block;
    margin: auto;
    width: 40px;
    height: 40px;
}
</style>