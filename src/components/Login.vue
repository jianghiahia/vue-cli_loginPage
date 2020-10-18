<template>
    <div class="login_container">
       <div class="login_box">
           <!-- 头像区域 -->
           <div class="avater_box">
               <img src="../assets/login.jpg" alt="">
           </div>
           <!-- 表单区域 -->
            <el-form ref="loginFormRef" label-width="0" :model="loginForm" :rules="loginFormRules" class="login_form">
                <!-- 用户名 -->
                <el-form-item prop="username">
                    <el-input  v-model="loginForm.username" prefix-icon="iconfont icon-user"></el-input>
                </el-form-item>
                <!-- 密码区 -->
                <el-form-item prop="password">
                    <el-input  v-model="loginForm.password" prefix-icon="iconfont icon-3702mima" type="password"></el-input>
                </el-form-item>
                <el-form-item class="btn">
                    <el-button type="primary" @click='preCheckForm'>登录</el-button>
                    <el-button type="info" @click='resetForm'>重置</el-button>
                </el-form-item>
            </el-form>
       </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            // 这是登录的表单对象
            loginForm:{
                username:'',
                password:''
            },
            loginFormRules:{
                // 一个数组是一个校验规则，数组名就是要验证的属性名，不可改
                username:[
                    { required: true, message: '请输入用户名', trigger: 'blur' },
                    { min: 3, max: 10, message: '长度在 3 到 10 个字符', trigger: 'blur' }
                 ],
                password:[
                    { required: true, message: '请输入密码', trigger: 'blur' },
                    { min: 3, max: 10, message: '长度在 3 到 10个字符', trigger: 'blur' }
                ]
            }
        }
    },
    methods:{
        resetForm:function(){
            this.$refs.loginFormRef.resetFields();
        },
        preCheckForm:function(){
            // async和await将promise对象转成数组
             this.$refs.loginFormRef.validate(async valid=>{
                //  valid是布尔值，存放验证结果
                if(!valid) return;
                // 先启动数据库+node app.js
                // data属性重命名为res        
                const { data: res} = await this.$http.post('login',this.loginForm);
                console.log(res);
                if(res.meta.status !== 200) return this.$message.error('登录失败'); 
             });
        }
    }
}
</script>

<style lang="less" scoped>
.login_container {
    background-color: #2b4b6b;
    height: 100%;
}
.login_box {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%,-50%);
    width: 450px;
    height: 300px;
    background-color: #fff;
    border-radius: 3px;
}
.avater_box {
    position: absolute;
    left: 50%;
    transform: translate(-50%,-50%);
    width: 130px;
    width: 130px;
    border: 1px solid #eee;
    border-radius: 50%;
    box-shadow: 0 0 10px #ddd;
    img { 
        width: 100%;
        height: 100%;
        border-radius: 50%;
    }  
}
.btn {
    display: flex;
    justify-content: flex-end;
}
.login_form {
    position: absolute;
    bottom: 10px;
    width: 100%;
    padding: 0 40px;
    box-sizing: border-box;
}
</style>