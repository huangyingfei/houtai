<template>
    <div class="detail">
        <!-- <h2>123123</h2> -->
        <div class="button" v-for="(item, i) in musics" :key="i" :data-id="item.id" @click="Addusers($event)">
            <div class="whole">
                <img :src="item.cover" lazy alt="" />
                <!-- <el-image :src="item.cover" lazy></el-image> -->
            </div>
            <div class="music">{{ item.name }}</div>
            <div class="query">{{ item.artistName }}</div>
        </div>

        <el-dialog :title="this.name" :visible.sync="users_unlock" width="900px">
            <span>
                <video :src="this.module" controls="controls"></video>
            </span>
            <span class="details">{{ this.desc }}</span>
            <span slot="footer" class="dialog-footer">
                <el-button @click="users_unlock = false">取 消</el-button>
                <el-button type="primary" @click="dialogVisible()">确 定</el-button>
            </span>
        </el-dialog>
    </div>
</template>

<script>
export default {
    name: 'detail',
    data() {
        return {
            musics: [],
            users_unlock: false,
            musicid: '',
            module: '', //MV简介
            name: '', //名字
            desc: '', //简称,
            addid: ''
        };
    },
    beforeMount() {
        this.scrobble();
    },
    methods: {
        Addusers(event) {
            this.users_unlock = true;
            // var target = event.target || window.event.srcElement;
            // console.log(event.currentTarget.dataset.id);

            //获取对应元素的id值
            this.addid = event.currentTarget.dataset.id;
            // console.log(this.addid);
            let songs = `?mvid=${this.addid}`;
            this.$axios
                .get(`http://musicapi.leanapp.cn/mv/detail` + songs, {
                    headers: {
                        // token: sessionStorage.getItem('token')
                        'Content-Type': 'application/json'
                    }
                })
                .then(res => {
                    console.log(res);
                    this.disks = res.data.data.brs;
                    this.name = res.data.data.name;
                    this.desc = res.data.data.desc;
                    this.module = this.disks['480'];
                    // console.log(this.disks);
                    // console.log(this.name);
                    // console.log(this.desc);
                    // console.log(this.module);
                });
        },
        dialogVisible() {
            this.users_unlock = false;
            this.scrobble();
        },
        scrobble() {
            let added = `?limit=100`;
            this.$axios
                .get(`http://musicapi.leanapp.cn/mv/first` + added, {
                    headers: {
                        // token: sessionStorage.getItem('token')
                        'Content-Type': 'application/json'
                    }
                })
                .then(res => {
                    // console.log(res);
                    this.musics = res.data.data;
                    // this.musicid = res.data.data.artistId;
                    // console.log(this.musicid);
                    console.log(this.musics);
                });
        }
    }
};
</script>

<style scoped>
.detail {
    width: 100%;
    height: 100%;
}
.button {
    width: 160px;
    height: 230px;
    /* background-color: red; */
    /* float: left; */
    display: inline-block;
    /* float: left; */
    margin-left: 40px;
    margin-bottom: 20px;
    vertical-align: middle;
}
.whole {
    width: 160px;
    height: 150px;
}
.whole img {
    width: 160px;
    height: 150px;
}
.music {
    width: 160px;
    height: 25px;
    /* text-align: center; */
    margin-top: 8px;
    font-size: 13px;
}
.query {
    width: 160px;
    height: 25px;
    color: #666;
}
.details {
    font-size: 18px;
    color: #333;
}
.el-dialog__title {
    line-height: 24px;
    font-size: 18px;
    /* color: #303133; */
    color: #c20c0c;
}
</style>
