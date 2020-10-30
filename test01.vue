<template>

    <div style="text-align:center;width:100%;height:100%;border:0px solid #008DCC;">
        <section>
            <el-row class="container">
                <el-col class="right">
                    <!--header -->
                    <el-col class="top" style="border:0px solid #008DCC;min-width: 1903px;">
                        <div class="navbar navbar-default navbar-fixed">
                            <div class="navbar-one" style="border:0px solid #008DCC;">
                                <a href="" class="navbar-brand" data-delay="99" @click="this.$router.push('/home');">
                                    <img class="aia-logo" src="../assets/day2/images/aiawhite-logo.png">
                                   <!-- <span id="logo-span">The Real Life Company</span> -->
                                </a>

                                <ul class="nav navbar-nav" data-user-menu="individual">
                                    <li class="dropdown" v-for="(item,index) in treeData">
                                        <router-link replace class="dropdown-toggle" data-toggle="dropdown"
                                                     data-hover="dropdown"
                                                     data-delay="10" aria-expanded="false" data-close-others="false"
                                                     tabindex="100"
                                                     :to="{path:item.path}">
                                            {{item.name}}
                                        </router-link>
                                        <div class="dropdown-menu" v-if="typeof(item.subTree)!='undefined'"
                                             style="width:100%;">
                                            <div class="container-fluid margin-bottom-xxl" style="width:100%">
                                                <div class="menu-row">
                                                    <div class="menu-title">
                                                        <h4>{{item.name}}</h4>
                                                    </div>
                                                    <div class="menu-line"></div>
                                                    <!-- <div class=""> -->
                                                        <div class="row">
                                                            <!-- <div class="col-sm-4" style="width:100%;"> -->
                                                                <ul class="list-unstyled" style="width:100%;">
                                                                    <li class="list-ele"
                                                                        v-for="(item,index) in item.subTree">
                                                                        <router-link replace class="dropdown-toggle h6"
                                                                                     data-hover="dropdown"
                                                                                     data-delay="100"
                                                                                     aria-expanded="false"
                                                                                     tabindex="100"
                                                                                     style="padding-left:28px"
                                                                                     :to="{path:item.path}">
                                                                            <div class="thumbnail"><img
                                                                                    src="../assets/day2/images/icons/claim_status_kr-prime1.png">
                                                                            </div>
                                                                            <div class="content">
                                                                                <h6 class="p3">{{item.name}}</h6>
                                                                                <!-- <p class="p3 bt5">Input your daily effort.</p>  -->
                                                                            </div>
                                                                        </router-link>
                                                                    </li>
                                                                </ul>
                                                            <!-- </div> -->
                                                        </div>
                                                    <!-- </div> -->
                                                </div>

                                            </div>
                                        </div>
                                    </li>
                                </ul>

                                <ul class="nav nav-icon navbar-tools">
                                    <li class="sub-icon one">
                                        <!--<a href="">-->
                                        <img src="../assets/images/user.png" border="0" style="width:34px;height:35px;">
                                        <label class="nav_right">{{this.displayName}}</label>
                                        <!--</a>-->
                                    </li>

                                    <li class="sub-icon two">
                                        <div class="selStyle" style="background-color:rgba(211,17,69,1);">
                                           						<span id="user-select-span">{{channel}}</span>
                                            <span id="user-select-span" v-if="channel === 'Banca'" style="font-size:14px;">{{branchAgentRoleSel}}</span>
                                            <el-select v-if="channel === 'Banca'"
                                                       v-model="branchAgentRoleSel"
                                                       @change="getCurrentBranchAgentCode($event)"
                                                       style="opacity:0.00001;left:0;position: absolute;top: -12px;"
                                                       placeholder=""
                                                       class="nav_right ">
                                                <el-option v-for="item in optionBranchAgentCodeListSum"
                                                           :key="item.value"
                                                           :label="item.label"
                                                           :value="item.value">
                                                </el-option>
                                            </el-select>
											
											<label v-if="channel === 'Agency'" class="nav_right">{{branchAgent}}</label>
                                        </div>

                                      <!--  <el-label v-if="channel === 'Agency'" class="nav_right">{{branchAgent}}
                                        </el-label>-->
                                        <!-- <a href="">
                                            <img width="16px" height="16px" src="../assets/images/time.png" border="0">
                                            <label class="nav_right">{{this.sysdate}}</label>
                                        </a>-->
                                    </li>

                                    <li class="sub-icon three">
                                        <!-- <a href="" style="font-size:24px;">
                                             <img width="16px" height="16px" src="../assets/images/logout.png" border="0">
                                             <label class="nav_right">EXIT</label>
                                         </a>-->

                                        <!-- <a href="" style="font-size:24px;"> -->
                                        <!-- <router-link to="/login">
                                            <img width="16px" height="16px" src="../assets/images/logout.png"
                                                    border="0">
                                            <label class="nav_right">EXIT</label>
                                        </router-link> -->
                                        <!-- </a> -->
                                        <a  style="font-size:24px;position:relative" >
                                            
                                            <div  @click="loginout">
                                                <img width="16px" height="16px" style="position:absolute;left:-20px;top:19px" src="../assets/images/logout.png"
                                                     border="0">
                                                <label class="nav_right" style="margin-left:3px;">EXIT</label>
                                            </div>
                                            
                                        </a>
                                    </li>
                                </ul>
                            </div>
                        </div>

                    </el-col>

                    <div class="bottom" style="border:0px solid blue; background-color:#F9F9F3;margin-top:60px;">
                        <section>
                            <el-col class="content-wrapper">
                                <transition name="fade" mode="out-in">
                                    <router-view :key="this.$route.path"></router-view>
                                </transition>
                            </el-col>
                        </section>
                    </div>
                </el-col>
            </el-row>
            <!-- <div class="navbar-fixed-spacer" style="border:0px solid red;"></div>-->
            <!-- <router-view></router-view> -->
        </section>
     
    </div>
</template>


<script>
  
    import "../store/jquery-3.4.1.js";
    import "../assets/bootstrap.min.js";
    import "../store/bootstrap-dropdown.js";

    //import menuone from "./tab/menuOne.vue";
    //import menuBar from "./tab/menu.vue";

    export default {
        data() {
            return {
                branchAgentRole:0,
				branchAgentRoleSel:'',
                baseUrl: domain.baseUrl,
                conheight: {height: ""},
                channel: "",
                treeName: "",
                SubList: [],
                timer: "",
                isCollapse: true,
                isLeftOpen: true,
                loginUser: "",
                displayName: "",
                agentCode: "",
                sysdate: "",
                date: new Date(),
                branchAgent: "",
                optionBranchAgentCodeList: [],
		        optionBranchAgentCodeListSum: [],
                treeData: [],
                userRole: ""
            };
        },
        created() {
            window.addEventListener("resize", this.getHeight);
            this.getHeight();
			this.getMenuList();
            $(document).ready(function () {
                // apply dropdownHover to all elements with the data-hover="dropdown" attribute
                $('[data-hover="dropdown"]').dropdownHover();
            });
        },
        methods: {
         
			loginout(){
                window.localStorage.clear();
                //window.location.href="https://login.microsoftonline.com/f7e0a917-60c8-4941-8b7c-5f1c7b3b0821/oauth2/logout?client_id=6c00d5b5-d3f8-452d-9bce-54a96ccd7972&post_logout_redirect_uri=https://10.212.145.66/LMS/logout";
                //window.location.href="https://login.microsoftonline.com/f7e0a917-60c8-4941-8b7c-5f1c7b3b0821/oauth2/logout?client_id=6c00d5b5-d3f8-452d-9bce-54a96ccd7972&post_logout_redirect_uri=https://aiasmartsit.aialife.com.lk/LMS/logout";
                //window.location.href="https://login.microsoftonline.com/f7e0a917-60c8-4941-8b7c-5f1c7b3b0821/oauth2/logout?client_id=6c00d5b5-d3f8-452d-9bce-54a96ccd7972&post_logout_redirect_uri=https://aiasmartsit.aialife.com.lk/LMS"; //sit 
                window.location.href="https://login.microsoftonline.com/f7e0a917-60c8-4941-8b7c-5f1c7b3b0821/oauth2/logout?client_id=6c00d5b5-d3f8-452d-9bce-54a96ccd7972&post_logout_redirect_uri=https://aiasmartuat.aialife.com.lk/LMS"; //uat
           },

            getHeight() {
                this.conheight.height =
                    window.innerHeight - window.innerHeight * 0.00001 + "px";
            },
            transformMenu() {
                this.isLeftOpen = !this.isLeftOpen;
            },
            getTreeName(data) {
                this.treeName = data;
                //console.log(this.treeName + "main");
            },
            getCurrentBranchAgentCode(value) {
                var underlineIndex01 = this.branchAgentRoleSel.lastIndexOf("_");
                //console.log('555:',this.branchAgentRoleSel);
                this.GLOBAL.newBranch = this.branchAgentRoleSel.substring(0, underlineIndex01);
                this.GLOBAL.newAgentCode = this.branchAgentRoleSel.substring(underlineIndex01 + 1, this.branchAgentRoleSel.length);
                for (var i = 0; i < this.optionBranchAgentCodeList.length; i++) {
                    if(this.branchAgentRoleSel == this.optionBranchAgentCodeList[i].label){
                        this.GLOBAL.newRole = this.optionBranchAgentCodeList[i].role;
                    }
                }
                localStorage.setItem('newBranch', JSON.stringify(this.GLOBAL.newBranch));
                localStorage.setItem('newAgentCode', JSON.stringify(this.GLOBAL.newAgentCode));
                localStorage.setItem('newRole', this.GLOBAL.newRole);
                if(this.GLOBAL.newRole === 'Admin') {
                	 this.getMenuList();
                	 this.$router.push({path: "/userManagement"})
                } else {
                	this.getMenuList();
                    this.$router.push({path: "/home"})
                    document.location.reload();
                }
            },
            getBranchAgentCodeList() {
                this.optionBranchAgentCodeList = [];
		        this.optionBranchAgentCodeListSum = []; 
                var branchAgentList = JSON.parse(localStorage.getItem("branchAgent"));
                var indexBranchAgent = "";
                var indexBranchAgentRole = "";
                var index = -1;
                var thisBranchAgent = "";
		        this.optionBranchAgentCodeListSum=branchAgentList.map(x=>{
                    let branchArray=x.split(':');
                    return {label:branchArray[0],value:branchArray[0]};
                })
                this.optionBranchAgentCodeList=branchAgentList.map(x=>{
                    let branchArray=x.split(':');
                    return {label:branchArray[0],value:branchArray[0],role:branchArray[1]};
                })
                let branchNameAndCode = JSON.parse(localStorage.getItem("branchNameAndCode"));
                // console.log(branchNameAndCode);
                for(let i in this.optionBranchAgentCodeListSum) {
                    let branchCode = this.optionBranchAgentCodeListSum[i].label.split('_')[0];
                    for(let j in branchNameAndCode) {
                        if(branchCode == branchNameAndCode[j].split(':')[1]){
                            this.optionBranchAgentCodeListSum[i].label = branchNameAndCode[j].split(':')[0] +'_'+this.optionBranchAgentCodeList[i].label.split('_')[1]
                            this.optionBranchAgentCodeListSum[i].value = this.optionBranchAgentCodeListSum[i].label
                            this.optionBranchAgentCodeList[i].label = branchNameAndCode[j].split(':')[0] +'_'+this.optionBranchAgentCodeList[i].label.split('_')[1]
                            this.optionBranchAgentCodeList[i].value = this.optionBranchAgentCodeList[i].label
                        }
                    }
                }
                console.log(this.optionBranchAgentCodeListSum);
		        if(
                    (   localStorage.getItem("newBranch")+'' != localStorage.getItem("thisBranch")+'' ||
                        localStorage.getItem("newAgentCode")+'' != localStorage.getItem("thisAgentCode")+''||
                        localStorage.getItem("newRole")+'' != localStorage.getItem("role"))+'' 
                        
                    ){ 
                        if(localStorage.getItem("newBranch") == '' || 
                        localStorage.getItem("newBranch")  == undefined || 
                        localStorage.getItem("newBranch")  == null){
                           
                            this.branchAgentRoleSel = this.optionBranchAgentCodeList[0].value;
                            let firstRole=this.optionBranchAgentCodeList[0].role;
                            localStorage.setItem('role', firstRole);
                            this.branchAgent = this.optionBranchAgentCodeList[0].value;
                            thisBranchAgent = this.branchAgent.split('_');
                            localStorage.setItem('thisBranch', JSON.stringify(thisBranchAgent[0]));
                            localStorage.setItem('thisAgentCode', JSON.stringify(thisBranchAgent[1]));
                        }else{  
                            localStorage.setItem('thisBranch', localStorage.getItem("newBranch"));
                            localStorage.setItem('thisAgentCode', localStorage.getItem("newAgentCode"));
                            localStorage.setItem('role', localStorage.getItem("newRole"));

                            var s1 = JSON.parse(localStorage.getItem("thisBranch"));
                            var s2 = JSON.parse(localStorage.getItem("thisAgentCode"));
                        
                            this.branchAgent = s1+'_'+s2;
                            thisBranchAgent = this.branchAgent.split('_');
                            this.branchAgentRoleSel = s1+'_'+s2;
                        }
		        }
                else{
                    localStorage.setItem('thisBranch', localStorage.getItem("thisBranch"));
                    localStorage.setItem('thisAgentCode', localStorage.getItem("thisAgentCode"));
                    localStorage.setItem('role', localStorage.getItem("role"));

                    var s1 = localStorage.getItem("thisBranch");
                    var s2 = localStorage.getItem("thisAgentCode");
                   
                    this.branchAgent = s1+'_'+s2;
                    thisBranchAgent = this.branchAgent.split('_');
                    
                    this.branchAgentRoleSel = s1+'_'+s2;
                }
                // console.log(this.optionBranchAgentCodeList);
            },
            getMenuList() {
				
                var arr = this;
                arr.getParam();
                arr.getBranchAgentCodeList();
                arr.getAgentCode();
                var param = {
                    lanId: this.loginUser,
                    agentCode: this.agentCode
                }
                this.$axios({
                    method: "POST",
                    url: this.baseUrl + "user/getMenuList",
                    contentType: "application/json;charset=utf-8",
                    data: param
                }).then(function (response) {
                    if (response.data.RESPONSE_CD == "0") {
                        arr.treeData = response.data.DATA;
                    } else {
                        alert(response.data.RESPONSE_DESC_ERR);
                        return;
                    }
                }).catch(function (error) {
                    console.log(error);
                });
            },
            getAgentCode() {
                let underlineArr=this.branchAgent.split('_');
                let colonArr=underlineArr[1].split(':');
                this.agentCode=colonArr[0];
            },
            getParam() {
	            this.loginUser = JSON.parse(localStorage.getItem("lanId"));
                this.displayName = JSON.parse(localStorage.getItem("displayName"));                
                
	            var branchAgentList = JSON.parse(localStorage.getItem("branchAgent"));
                this.channel = JSON.parse(localStorage.getItem("channel"));
                
            },
        },
        components: {
        
            //menuone,
            //menuBar
        }
    };
</script>

<style lang="scss">
    .container {
        min-width: 768px;
        float: left;
        width: 100%;
    }

    .main_board {
        width: 100%;
    }

    .left {
        width: 15%;

        float: left;
        overflow: hidden;
        position: relative;
    }

    .leftHide {
        width: 0%;

        float: left;
        overflow: hidden;
        position: relative;

    }

    .right {
        margin: 0px;
        padding: 0px;
        border: 0px;
        overflow: auto;
        float: right;
        width: 100%;
    }

    .top {
        // height: 8%;
        // float: top;
        border-bottom: 1px solid #ebedf0;
        z-index: 1000;
        position:fixed;
    }

    .bottom {
        float: bottom;
        height: 100%;
        width: 100%;
    }

    .treeStyle {
        background-color: #2a2924 !important;
        font-size: 20px;
        font-weight: bold;
        color: white;
        margin-top: 30px;
        text-align: left;
        white-space: nowrap;
        cursor: pointer;
    }


    .el-select-dropdown__list .el-select-dropdown__item:hover,.el-select-dropdown__list .hover{
        background-color: rgb(211, 17, 69) !important;
        color: rgb(255, 255, 255) !important;
    }

    #user-select-span{
        font-size: 16px;
        font-family: "AIATitle",sans-serif;         
    }
</style>

<style scoped>

    .nav_right {
        font-size: 16px;
        font-family: "AIATitle";
        font-weight: 100;
        color: rgba(255, 255, 255, 1);
        margin-left:0;
    }

    * {
        margin: 0;
    }

    .margin-bottom-3xl {
        margin-bottom: 0px;
    }

    .navbar {
        /* height: 88px; */
        height:60px;
        background-color: #d31145;
    }

    .navbar-brand {
        left: 24px;
        display:flex;
        align-items: center;
    }

    #logo-span{
        margin-left: 6px;
      font-family: "AIATitle";
        overflow: hidden;
        letter-spacing: 0.6px;
        font-weight: 200;
        width: 68px;
        word-break: break-all;
        line-height: 15px;
        font-size: 12px;
    }

    @media (min-width: 1025px) {
        .navbar-brand {
            /* left: 0px; */
            right: auto;
            padding-top: 0px;
            padding-bottom: 0px;
            text-align: left;
        }
    }

    .navbar,
    .navbar-default,
    .navbar-fixed {
        padding: 0;
    }

    .navbar-one {
        /* height: 88px; */
        height: 60px;
        padding-left: 0px;
        padding-right: 0px;
    }

    .aia-logo {
        /* padding: 20px 48px 20px 60px; */
        margin: 0;
       height: 47px;
        width:47px;
    }

    @media (max-width: 1024px) {
        .aia-logo {
            padding: 0;
        }
    }

    @media (max-width: 768px) {
        .nav-icon .one {
            display: none;
        }

        .nav-icon .three {
            padding-right: 25px;
        }
    }

    .menu-row{
        padding-top:32px;
        width:100%;
        display:flex;
    }

    .menu-row .menu-title{
        flex: 1;
    }

    .menu-row .menu-line{
        width: 1px;
        background: #DBDAD4;
    }

    .row{
        flex:4;
    }

    ul.navbar-nav {
        height: 100%;
        margin-left: 190px;
    }

    .navbar-nav > li > a {
        display: table-cell;
        vertical-align: middle;
    }

    #work {
        background-color: #faf9f3;
        height: 300px;
        margin: 0;
    }

    .col-sm-3{
        padding:0;
    }

    .dropdown-menu h4 {
        font-family: "AIATitle";
        font-size: 32px;
        margin-bottom: 26px;
        color: #cb1542;
    }

    .dropdown-menu p {
       font-family: "AIATitle";
        font-size: 16px;
        line-height: 22px;
    }

    #work .col-md-3 {
        padding: 0;
    }

    #work .subtitle {
        display: inline-block;
    }

    .dropdown-menu img {
        width: 32px;
        height: 32px;
        padding: 0;
    }

    .left-part h4 {
        padding: 0;
        font-size: 30px;
       font-family: "AIATitle";
    }

    .left-part p {
        width: 210px;
       font-family: "AIATitle";
        font-size: 16px;
        line-height: 22px;
        color: #5e4e4f;
        padding: 0;
    }

    .right-part .icon {
        display: inline-block;
    }

    .nav > li > a {
    font-family: "AIATitle";
        font-size: 20px;
        line-height: 20px;
        color: white;
        margin: 0;
    }

    .nav.nav-icon.navbar-tools label {
        /* height: 88px; */
        line-height: 60px;
        display: inline-block;
        vertical-align: middle;
        text-align: center;
        margin-left: 10px;
        font-size:14px;
    }

    .nav-icon {
        margin: 0;
        display: inline-block;
       position: absolute;
        right: 30px;

        /* padding-right: 61px;*/
        height: 100%;
    }

    .sub-icon {
        font-family: "AIATitle";
        height: 100%;
        margin: 0 24px;
    }

    .sub-icon > img {
        margin: 0;
    }


    .navbar-nav {
        display: none
    }

    .navbar-nav > li {
        height: 100%;
        display: inline-block;
    }

    .navbar-nav > li > a {
        /* padding-top: 24px;
        padding-bottom: 24px;
        padding-left: 24px;
        padding-right: 24px; */
        text-decoration: none
    }

    .navbar-nav > li.open {
        background-color: #B8123E;
        background-repeat: no-repeat;
        background-position: 50% 100%;
        background-image: url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABAAAAAIAgMAAAH6H6eSAAAADFBMVEX////////////////1pQ5zAAAAA3RSTlMAfX7XWp9eAAAANElEQVQI1wXB0Q1AUAwAwMOHhCXeBhYQRq9ROoIBpHVn4MJpSXeZzNvjeMXHmohi707RXT/7CQ+qIm3ujgAAAABJRU5ErkJggg==)
    }

    .navbar-nav > .dropdown.open {
        position: static
    }

    @media (min-width: 1025px) {
        .navbar-nav {
            display: inline-block;
            margin-left: 164px
        }

    }

    @media (min-width: 1025px) {
        .navbar-left {
            float: left !important
        }

        .navbar-right {
            float: right !important;
            margin-right: -22px
        }

        .navbar-right ~ .navbar-right {
            margin-right: 0
        }

    }

    .navbar-default {
        background-color: #D31145;
        border-color: transparent
    }

    .navbar-default .navbar-nav > li > a {
        color: #fff;
        font-size: 16px;
        font-family: "AIATitle";
        padding: 0 24px;
    }

    .navbar-default .navbar-nav[data-user-menu="business"],
    .navbar-default .navbar-nav[data-user-menu="sales"] {
        display: none
    }

    .navbar-default .navbar-nav .open .dropdown-menu {
        width: 100%;
        background-color: #fff;
        box-shadow: 0 6px 12px rgba(0, 0, 0, 0.175)
    }

    .navbar-default .navbar-nav .open .dropdown-menu .col-half {
        display: inline-block
    }

    .navbar-default .navbar-nav .open .dropdown-menu .border-left {
        border-left: 1px solid #DBDAD4;
        border-collapse: collapse
    }

    .navbar-default .navbar-nav .open .dropdown-menu .border-right {
        border-right: 1px solid #DBDAD4;
        border-collapse: collapse
    }

    .navbar-default .navbar-nav .open .dropdown-menu .thumbnail {
        position: absolute
    }

    .navbar-default .navbar-nav .open .dropdown-menu .content {
        display: inline-block;
        padding-left: 40px;
        margin-top: 5px;
    }

    .navbar-default .navbar-nav .open .dropdown-menu .list-ele {
        height: 84px;
        width:31.9%;
        margin: 0 10px;
        cursor: pointer;
    }

    .navbar-default .navbar-nav .open .dropdown-menu .list-ele a {
        display: block
    }

    .navbar-default .navbar-nav .open .dropdown-menu .list-ele a:focus h6,
    .navbar-default .navbar-nav .open .dropdown-menu .list-ele a:focus p,
    .navbar-default .navbar-nav .open .dropdown-menu .list-ele a:hover h6,
    .navbar-default .navbar-nav .open .dropdown-menu .list-ele a:hover p {
        color: #D31145
    }

    .navbar-default .navbar-nav .open .dropdown-menu .hover-red:hover {
        color: #D31145
    }

    .navbar-default .navbar-nav .open .dropdown-menu .close-nav-dropdown-btn {
        position: absolute;
        top: 20px;
        right: 58px
    }

    .navbar-tools > li {
        /*float: left*/
    }

    .navbar-tools > li > .dropdown-toggle {
        min-width: 44px;
        padding-top: 18px;
        padding-bottom: 18px;
        cursor: pointer;
    }

    @media (min-width: 1025px) {
        .navbar-tools > li > .dropdown-toggle {
            padding-top: 22px;
            padding-bottom: 22px
        }

    }

    @media (min-width: 768px) {
        .navbar-tools > li .navbar-tool-label {
            padding-top: 10px;
            padding-bottom: 10px
        }

    }

    .navbar-tools > li .navbar-tool-label > svg {
        display: block
    }

    .navbar-tools > li.open {
        background-repeat: no-repeat;
        background-position: 50% 100%;
        background-image: url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABAAAAAIAgMAAAH6H6eSAAAADFBMVEX////////////////1pQ5zAAAAA3RSTlMAfX7XWp9eAAAANElEQVQI1wXB0Q1AUAwAwMOHhCXeBhYQRq9ROoIBpHVn4MJpSXeZzNvjeMXHmohi707RXT/7CQ+qIm3ujgAAAABJRU5ErkJggg==)
    }

    @media (min-width: 768px) {
        .navbar-tools > li.open .navbar-tool-label {
            background-color: #B8123E
        }

    }

    .navbar-tools .navbar-text {
        line-height: 24px
    }

    .navbar-tools .navbar-btn-dark {
        background-color: #B8123E
    }

    .navbar-tools .dropdown.open {
        position: static
    }

    @media (min-width: 768px) {
        .navbar-tools .dropdown.open {
            position: relative
        }

    }

    .navbar-tools .open .dropdown-menu {
        padding-left: 0;
        list-style: none;
        width: 100%;
        background-color: #fff
    }

    .navbar-tools .open .dropdown-menu > li:first-child {
        margin-top: 4px
    }

    .navbar-tools .open .dropdown-menu > li > a {
        padding-top: 14px;
        padding-bottom: 14px;
        padding-left: 24px;
        padding-right: 24px
    }

    .navbar-tools .open .dropdown-menu > li > a:active h6,
    .navbar-tools .open .dropdown-menu > li > a:hover h6,
    .navbar-tools .open .dropdown-menu > li > a:focus h6 {
        color: #22A8DA
    }

    .navbar-tools > .navbar-search-btn.open {
        background-image: url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABAAAAAICAMAAADHqI+lAAAADFBMVEW4Ej64Ej64Ej64Ej58QEVpAAAAA3RSTlMAfX7XWp9eAAAAKElEQVR4AVXIsQ0AAAiEQPX339mGfAIdN7Q3ahPJJRUe0SM8okf6yAMj/ADHW2M1hAAAAABJRU5ErkJggg==)
    }

    @media (min-width: 768px) {
        .navbar-tools > .navbar-search-btn.open {
            position: static
        }

    }

    .navbar-tools .open .lang-dropdown-menu {
        box-shadow: 0 6px 12px rgba(0, 0, 0, 0.175)
    }

    @media (min-width: 768px) {
        .navbar-tools .open .lang-dropdown-menu {
            width: auto;
            left: -37px
        }

    }

    .navbar-tools .open .contact-dropdown-menu {
        box-shadow: 0 6px 12px rgba(0, 0, 0, 0.175)
    }

    @media (min-width: 768px) {
        .navbar-tools .open .contact-dropdown-menu {
            width: auto;
            left: auto;
            right: -44px
        }

    }

    .navbar-tools .open .search-dropdown-menu {
        height: 60px
    }

    .navbar-tools .open .search-dropdown-menu > li:first-child {
        margin-top: 0
    }

    .navbar-tools .open .search-dropdown-menu .search-bar {
        background-color: #B8123E
    }

    @media (min-width: 768px) {
        .navbar-tools .open .search-dropdown-menu .search-bar {
            padding-left: 12px;
            padding-right: 12px
        }

    }

    @media (min-width: 1025px) {
        .navbar-tools .open .search-dropdown-menu .search-bar {
            height: 120px;
            padding-left: 48px;
            padding-right: 48px
        }

    }

    .navbar-tools .search-col-right {
        float: right;
        padding-top: 10px;
        padding-bottom: 10px;
        width: 80px;
        font-size: 0
    }

    @media (min-width: 768px) {
        .navbar-tools .search-col-right {
            width: 194px;
            padding-top: 40px;
            padding-bottom: 40px
        }

    }

    .navbar-tools .search-col-right > .btn,
    .navbar-tools .search-col-right > .btn-primary,
    .navbar-tools .search-col-right > .btn-inline-search,
    .navbar-tools .search-col-right > .btn-secondary,
    .navbar-tools .search-col-right > .btn-toggle,
    .navbar-tools .search-col-right > .btn-tertiary,
    .navbar-tools .search-col-right > .btn-image {
        vertical-align: top
    }

    .navbar-tools .search-col-left {
        padding-right: 24px;
        overflow: hidden
    }

    .navbar-tools .go-btn {
        min-width: 0;
        width: 40px;
        padding-left: 0;
        padding-right: 0
    }

    @media (min-width: 768px) {
        .navbar-tools .go-btn {
            width: 124px;
            margin-right: 24px
        }

    }

    .navbar-tools .dismiss-search-btn {
        min-width: 0;
        padding-left: 8px;
        padding-right: 8px;
        background-color: #B8123E
    }

    .navbar-tools .dismiss-search-btn > svg {
        display: block
    }

    .navbar-tools .search-input {
        width: 100%;
        margin-left: 0;
        padding-top: 0;
        padding-bottom: 0;
        border: 0;
        background-color: #B8123E;
        color: #fff;
        height: 60px;
        line-height: 60px
    }

    .navbar-tools .search-input::-moz-placeholder {
        color: #D31145;
        opacity: 1
    }

    .navbar-tools .search-input:-ms-input-placeholder {
        color: #D31145
    }

    .navbar-tools .search-input::-webkit-input-placeholder {
        color: #D31145
    }

    @media (min-width: 768px) {
        .navbar-tools .search-input {
            height: 120px;
            line-height: 120px
        }

    }

    .navbar-tools .search-input:focus {
        color: #fff;
        border: none
    }

    .navbar-tools .search-bar-btn {
        width: 44px;
        background-image: none;
        background-color: transparent;
        border: 0
    }

    .selStyle {
        /* background-color: rgba(211, 17, 69, 1);
        font-size: 20px;
        font-family: AIAEverestCondensedMedium; */
        color: rgba(255, 255, 255, 1);
        line-height: 59px;
        height:100%;
    }



    /* .el-select-dropdown  .el-scrollbar{
        background: rgb(211, 17, 69);
    } */


</style>

