<template>
    <div>
        <h2>测测测</h2>
        <el-button type="success" @click="playlist">请求测试</el-button>
    </div>
</template>

<script>
export default {
    data() {
        return {};
    },
    methods: {
        playlist() {
            //  let added = `?limit=100`;
            this.$axios
                .get(`http://musicapi.leanapp.cn/homepage/block/page`, {
                    headers: {
                        // token: sessionStorage.getItem('token')
                        'Content-Type': 'application/json'
                    }
                })
                .then(res => {
                    console.log(res);
                });
        }
    }
};
</script>
