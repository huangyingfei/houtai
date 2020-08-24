<template>
    <div class="overview">
        <el-table
            style="width:100%;overflow-x: auto;overflow-y: auto;"
            :data="list_move"
            v-loading="loadFlag"
            :element-loading-text="page_data.loadTxt"
            empty-text="没有数据~"
            stripe
            border
        >
            <el-table-column prop="id" label="ID" align="center"> </el-table-column>

            <el-table-column prop="artistName" label="歌手" align="center"> </el-table-column>

            <el-table-column prop="artists" label="MV歌手" align="center">
                <template slot-scope="scope">
                    <div v-for="(item, i) in scope.row.artists" :key="i">
                        <!-- <span>{{ item.id }}</span
                        ><br /> -->
                        <span>{{ item.name }}</span>
                    </div>
                </template>
            </el-table-column>

            <el-table-column label="图片" align="center">
                <template slot-scope="scope">
                    <el-image class="table-td-thumb" :src="scope.row.cover" :preview-src-list="[scope.row.cover]"></el-image>
                </template>
            </el-table-column>

            <el-table-column prop="name" label="MV名称" align="center"> </el-table-column>
        </el-table>
        <!-- <el-pagination
              background
              layout="prev, pager, next"
               @current-change="handleCurrentChange"

               :page-size="pagesize"         
               :total="total"
              >
          </el-pagination> -->
    </div>
</template>

<script>
export default {
    name: 'overview',
    data() {
        return {
            page_data: {
                loadTxt: '请求列表中'
            },
            loadFlag: false, // 加载flag
            list_move: [],
            total: 1, //初始页
            pagesize: 1 //    每页的数据
        };
    },

    beforeMount() {
        this.listenadd();
    },
    methods: {
        listenadd() {
            this.$axios
                .get(`http://musicapi.leanapp.cn/top/mv`, {
                    headers: {
                        // token: sessionStorage.getItem('token')
                        'Content-Type': 'application/json'
                    }
                })
                .then(res => {
                    console.log(res);
                    this.list_move = res.data.data;
                    //    console.log(this.list_move)
                });
        },
        //分页点击
        handleCurrentChange(val) {
            this.page = val;
            this.getAdded();
        }
    }
};
</script>

<style scoped>
.table-td-thumb {
    display: block;
    margin: auto;
    width: 80px;
    height: 80px;
}
</style>
