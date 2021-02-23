<template>
    <div class="page">
        <div class="background">
            <el-container>
                <el-header>
                    <el-row :gutter="20">
                        <el-col :span="3">
                            <img src="../assets/img/logo.png" />
                        </el-col>
                        <el-col :span="15">
                            <img />
                        </el-col>
                        <el-col :span="3">
                            <el-link @click="home">LOG IN</el-link>
                        </el-col>
                        <el-col :span="3">
                            <el-link href="https://myefound.org/">MYE</el-link>
                        </el-col>
                    </el-row>
                </el-header>
                <el-main></el-main>
            </el-container>
        </div>
        <div class="courses_bg">
            <h1 style="font-family: monaco; font-size: 30px">
                popular courses
            </h1>

            <el-row :gutter="20">
                <el-col :span="6">
                    <el-card :body-style="{ padding: '20px' }">
                        <img src="../assets/img/python.jpg" class="image" />
                        <div class="courses_txt" style="padding: 10px">
                            <span>courses</span>
                        </div>
                    </el-card>
                </el-col>
                <el-col :span="6">
                    <el-card :body-style="{ padding: '20px' }">
                        <img src="../assets/img/python.jpg" class="image" />
                        <div class="courses_txt" style="padding: 10px">
                            <span>courses</span>
                        </div>
                    </el-card>
                </el-col>
                <el-col :span="6">
                    <el-card :body-style="{ padding: '20px' }">
                        <img src="../assets/img/python.jpg" class="image" />
                        <div class="courses_txt" style="padding: 10px">
                            <span>courses</span>
                        </div>
                    </el-card>
                </el-col>
                <el-col :span="6">
                    <el-card :body-style="{ padding: '20px' }">
                        <img src="../assets/img/python.jpg" class="image" />
                        <div class="courses_txt" style="padding: 10px">
                            <span>courses</span>
                        </div>
                    </el-card>
                </el-col>
            </el-row>
        </div>
    </div>
</template>

<script>
export default {
    data: function() {
        var checkpassword = (rule, value, callback) => {
            if (value === '') {
                callback(new Error('Please enter the password again'));
            } else if (value !== this.signup.password) {
                callback(new Error('The two passwords are inconsistent!'));
            } else {
                callback();
            }
        };
        return {
            isLogin: true,
            login: {
                username: '',
                password: ''
            },
            signup: {
                username: '',
                password: '',
                password_copy: ''
            },
            signUpRules: {
                username: {
                    required: true,
                    message: 'id can not be empty',
                    trigger: 'blur'
                },
                password: {
                    required: true,
                    message: 'id can not be empty',
                    trigger: 'blur'
                },
                password_copy: {
                    validator: checkpassword,
                    trigger: 'blur'
                }
            }
        };
    },
    methods: {
        home() {
            this.$router.push('/login');
        },
        GoLogin() {
            if (!this.isLogin) {
                this.isLogin = true;
            } else {
                this.$refs.login.validate(valid => {
                    if (valid) {
                        this.$axios({
                            method: 'post',
                            url: '/api/user/login',
                            data: this.login
                        }).then(resouce => {
                            console.log(resouce);
                            let res = resouce.data;
                            if (res == 'ok') {
                                this.$message.success('success');
                                this.$router.push('userhome');
                            } else {
                                this.$message.error('failed');
                            }
                        });
                    }
                });
            }
        },
        GoSignup() {
            if (this.isLogin) {
                this.isLogin = false;
            } else {
                this.$refs.signup.validate(valid => {
                    if (valid) {
                        this.$axios({
                            method: 'post',
                            url: '/api/user/signup',
                            data: this.signup
                        }).then(res => {
                            if (res.data == 'ok') {
                                this.$message.success('Sign Up Success');
                                this.isLogin = true;
                                this.login.username = this.signup.username;
                                this.login.password = this.signup.password;
                            } else {
                                this.$message.error('Sign Up Error');
                            }
                        });
                    }
                });
            }
        }
    }
};
</script>

<style lang="postcss" scoped>
.page {
    min-width: 1024px;
    min-height: 768px;
}
.background {
    background-color: beige;
    background-image: url(../assets/img/slider.jpg);
    background-size: cover;
    /* background-size: 100%; */
}
.courses_bg {
    margin-left: 10%;
    margin-top: 5%;
    width: 80%;
}
.el-header {
    text-align: right;
    line-height: 100px;
}
.el-main {
    height: 700px;
}
.el-link {
    width: 120px;
    height: 50px;
    font-family: helvetica;
    font-size: 25px;
}

.image {
    width: 100%;
    display: block;
}

.login-wrap {
    position: absolute;
    min-width: 1024px;
    width: 100%;
    top: 25%;
}
.form-login {
    position: absolute;
    right: 8%;
    width: 380px;
    border-radius: 5px;
    background: #dcdfe68e;
}
.form-title {
    width: 100%;
    line-height: 100px;
    text-align: center;
    font-family: helvetica;
    font-size: 35px;
    font-weight: 600;
    color: rgb(7, 116, 116);
    border-bottom: 2px solid rgb(151, 148, 148);
}
.form-smalltitle {
    width: 100%;
    padding-top: 0;
    padding-bottom: 20px;
    line-height: 30px;
    text-align: center;
    font-family: helvetica;
    font-size: 30px;
    font-weight: 600;
    color: rgb(12, 122, 8);
}
.courses_txt {
    font-family: monaco;
}
</style>
