<template>
  <div class="layout" :class="{'layout-hide-text': spanLeft < 5}">
    <template v-if="login">
        <Row type="flex">
            <i-col :span="spanLeft" class="layout-menu-left">
                <Menu theme="dark" width="auto" @on-select="itemChange" :active-name="activeName">
                    <div class="layout-logo-left" style="color:#bfcbd9;font-size:22px;font-weight:bold;">博客后台管理系统</div>
                        <Menu-item name="home">
                            <Icon type="android-home" :size="20"></Icon>
                            <span class="layout-text" style="color:#bfcbd9">首页</span>
                        </Menu-item>
                    
                        <Menu-item name="user">
                            <Icon type="transgender" :size="20"></Icon>
                            <span class="layout-text">用户</span>
                        </Menu-item>
                    
                        <Menu-item name="blog">
                            <Icon type="compose" :size="20"></Icon>
                            <span class="layout-text">日志</span>
                        </Menu-item>
                    
                        <Menu-item name="cate">
                            <Icon type="ios-cog" :size="20"></Icon>
                            <span class="layout-text">分类</span>
                        </Menu-item>
                    
                        <Menu-item name="comment">
                            <Icon type="chatbubble-working" :size="20"></Icon>
                            <span class="layout-text">评论</span>
                        </Menu-item>
                    
                        <Menu-item name="upload">
                            <Icon type="upload" :size="20"></Icon>
                            <span class="layout-text">上传</span>
                        </Menu-item>
                    
                        <Menu-item name="chart">
                            <Icon type="ios-analytics" :size="20"></Icon>
                            <span class="layout-text">图表</span>
                        </Menu-item>
                    
                </Menu>
            </i-col>
            <i-col :span="spanRight">
                <div class="layout-header">
                    <i-button type="text" @click="toggleClick">
                        <Icon type="navicon" size="32"></Icon>
                    </i-button>
                </div>
                <div class="layout-breadcrumb">
                    <Breadcrumb>
                        <Breadcrumb-item href="#">首页</Breadcrumb-item>
                        <Breadcrumb-item href="#">应用中心</Breadcrumb-item>
                        <Breadcrumb-item>某应用</Breadcrumb-item>
                    </Breadcrumb>
                </div>
                <div class="layout-content">
                    <div class="layout-content-main">
                      <router-view></router-view>
                    </div>
                </div>
                <div class="layout-copy">
                    2011-2016 &copy; TalkingData
                </div>
            </i-col>
        </Row>
      </template>
      <template v-else>
        <Form ref="formValidate" :model="formValidate" :rules="ruleValidate" :label-width="80" style="background:#d7dde4;"> 
            <Modal v-model="model" class-name="vertical-center-modal" :mask-closable="false">
                <p slot="header" style="color:#39f;text-align:center;font-size:20px;">
                    <span>后台管理系统</span>
                </p>
                <div slot="footer" style="padding:0"></div>
                <Form-item label="用户名" prop="name">
                    <Input v-model="formValidate.name" style="position:absolute;top:-25px;"></Input>
                </Form-item>
                <Form-item label="密码" prop="pwd" style="margin:40px 0;">
                    <Input v-model="formValidate.pwd" style="position:absolute;top:-25px;"></Input>
                </Form-item>
                <p style="margin-bottom:10px;">Tips：***用户名，密码随便填***</p>
                <Form-item>
                    <Button type="primary" @click="handleSubmit('formValidate')">提交</Button>
                    <Button type="ghost" @click="handleReset('formValidate')" style="margin-left: 8px">重置</Button>
                </Form-item>
            </Modal>
        </Form>
      </template>
    </div>
</template>

<script>
import {mapGetters,mapActions} from 'vuex';
var CryptoJs = require('crypto-js');
export default {
  data () {
      return {
          model:true,
          spanLeft: 5,
          spanRight: 19,
          activeName:'home',
          formValidate:{
            name:'',
            pwd:''
          },
          ruleValidate: {
            name: [
                { required: true, message: '姓名不能为空', trigger: 'blur' }
            ],
            
            pwd: [
                { required: true, message: '请输入个人介绍', trigger: 'blur' }
            ]
        }
      }
    },
  computed: {
      iconSize () {
          return this.spanLeft === 5 ? 14 : 24;
      },
      ...mapGetters(['login'])
  },
  methods: {
      ...mapActions(['checklogin']),
      toggleClick () {
          if (this.spanLeft === 5) {
              this.spanLeft = 2;
              this.spanRight = 22;
          } else {
              this.spanLeft = 5;
              this.spanRight = 19;
          }
      },
      handleSubmit(name){
        var _this = this;
        _this.formValidate.pwd = CryptoJs.MD5(_this.formValidate.pwd,{assign:true}).toString();
        this.$refs[name].validate((valid) => {
            if (valid) {
                _this.$http.post('http://localhost:3000/users/login',_this.formValidate).then(res=>{
                    _this.checklogin(true);
                })
            }
        })    
      },
      itemChange(name){        
        this.activeName=name;
        this.$router.push({name:name});
      }
  }
}
</script>

<style lang="scss">
  body,html,.ivu-row-flex,.layout{
      height: 100%;
    }
  .layout{
        border: 1px solid #d7dde4;
        background: #f5f7f9;
        position: relative;
        border-radius: 4px;
        overflow: hidden;
    }
    .layout-breadcrumb{
        padding: 10px 15px 0;
    }
    .layout-content{
        min-height: 80%;
        margin: 15px;
        overflow: hidden;
        background: #fff;
        border-radius: 4px;
    }
    .layout-content-main{
        padding: 10px;
    }
    .layout-copy{
        text-align: center;
        padding: 10px 0 20px;
        color: #9ea7b4;
    }
    .layout-menu-left{
        background: #464c5b;
    }
    .layout-header{
        height: 60px;
        background: #fff;
        box-shadow: 0 1px 1px rgba(0,0,0,.1);
    }
    .layout-logo-left{
        width: 90%;
        height: 30px;
        background: #5b6270;
        border-radius: 3px;
        margin: 15px auto;
    }
    .layout-ceiling-main a{
        color: #9ba7b5;
    }
    .layout-hide-text .layout-text{
        display: none;
    }
    .ivu-col{
        transition: width .2s ease-in-out;
    }
    .active li span{
        color: #39f;
    }
    .active li{
        background: #313540;
    }
    .vertical-center-modal{
        display: flex;
        align-items: center;
        justify-content: center;

        .ivu-modal{
            top: 0;
        }
    }
    .ivu-modal-content .ivu-modal-footer{
        padding:0;
    }
  
    
</style>
