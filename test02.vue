<template>
    <div class="bottom-page">
        <div class="button-blue" @click="AddLeadsInfo">
            <!-- <table>
                <tr>
                    <td>
                        <img class="addimag" src="../../assets/images/add.png" border="0">
                    </td>
                    <td width="5px;">
                    </td>
                    <td>
                        <span class="add-button ">ADD</span>
                    </td>
                </tr>
            </table> -->
            <img class="add-img" src="../../assets/images/add.png" border="0">
            <span class="add-button">ADD</span>
        </div>

        <div class="table-list">
            <el-table
                :data="calculateTableData"
                :row-style="tableRowStyle"
                :header-cell-style="tableHeaderColor"
                tooltip-effect="dark"
                ref="filterTable"
                empty-text="No Data Found"
                @sort-change="sortFieldChange"
                @filter-change="setFilters">
                <el-table-column prop="Name" label="Lead Name" sortable="custom">
                    <template slot="header" slot-scope="scope">
                        <label>Lead Name</label>
                        <i class="el-icon-search search-button" v-show="!columns[0].isSearch" @click.stop="columns[0].isSearch=true;"></i>
                        <div v-show="columns[0].isSearch" @click.stop="stopEvent($event)" class="input-div">
                            <el-input placeholder="Please input." size="small" v-model="columns[0].searchInput" @input="filterTableData"></el-input>
                            <el-button type="text" @click.stop="cancelFilter(0)">Cancel</el-button>
                        </div>
                    </template>
                    <template slot-scope="scope">
                        {{scope.row.Name}}
                        <i class="el-icon-lock" v-if="scope.row.IS_DELETED==2"
                           style="color:red">
                        </i>
                    </template>
                </el-table-column>
                <el-table-column prop="Phone" label="Contact No. Mobile" sortable="custom">
                    <template slot="header" slot-scope="scope">
                        <label>Contact No. Mobile</label>
                        <i class="el-icon-search search-button" v-show="!columns[1].isSearch" @click.stop="columns[1].isSearch=true"></i>
                        <div v-show="columns[1].isSearch" @click.stop="stopEvent($event)" class="input-div">
                            <el-input  placeholder="Please input." size="small" v-model="columns[1].searchInput" @input="filterTableData"></el-input>
                            <el-button type="text" @click.stop="cancelFilter(1)">Cancel</el-button>
                        </div>
                    </template>
                </el-table-column>
                <el-table-column prop="Gender" label="Gender" sortable="custom" >
                    <template slot="header" slot-scope="scope">
                        <label>Gender</label>
                        <i class="el-icon-search search-button" v-show="!columns[2].isSearch" @click.stop="columns[2].isSearch=true"></i>
                        <div v-show="columns[2].isSearch" @click.stop="stopEvent($event)" class="input-div">
                            <el-input placeholder="Please input." size="small" v-model="columns[2].searchInput" @input="filterTableData"></el-input>
                            <el-button type="text" @click.stop="cancelFilter(2)">Cancel</el-button>
                        </div>
                    </template>
                </el-table-column>
                <el-table-column prop="LEADS_CALL_DATE" label="Leads Call Date" sortable="custom"
                                 :formatter="leadsCallDataFormat">
                    <template slot="header" slot-scope="scope">
                        <label>Leads Call Date</label>
                        <i class="el-icon-search search-button" v-show="!columns[3].isSearch" @click.stop="columns[3].isSearch=true"></i>
                        <div v-show="columns[3].isSearch" @click.stop="stopEvent($event)" class="input-div">
                            <el-input placeholder="Please input." size="small" v-model="columns[3].searchInput" @input="filterTableData"></el-input>
                            <el-button type="text" @click.stop="cancelFilter(3)">Cancel</el-button>
                        </div>
                    </template>
                </el-table-column>
                <el-table-column prop="UPDATE_DATE" label="Last Updated Date" sortable="custom"
                                 :formatter="dataFormat">
                    <template slot="header" slot-scope="scope">
                        <label>Last Updated Date</label>
                        <i class="el-icon-search search-button" v-show="!columns[4].isSearch" @click.stop="columns[4].isSearch=true"></i>
                        <div v-show="columns[4].isSearch" @click.stop="stopEvent($event)" class="input-div">
                            <el-input placeholder="Please input." size="small" v-model="columns[4].searchInput" @input="filterTableData"></el-input>
                            <el-button type="text" @click.stop="cancelFilter(4)">Cancel</el-button>
                        </div>
                    </template>                                 
                </el-table-column>
                <el-table-column prop="ALLOCATION_TYPE" label="Allocation Type" sortable="custom">
                    <template slot="header" slot-scope="scope">
                        <label>Allocation Type</label>
                        <i class="el-icon-search search-button" v-show="!columns[5].isSearch" @click.stop="columns[5].isSearch=true"></i>
                        <div v-show="columns[5].isSearch" @click.stop="stopEvent($event)" class="input-div">
                            <el-input placeholder="Please input." size="small" v-model="columns[5].searchInput" @input="filterTableData"></el-input>
                            <el-button type="text" @click.stop="cancelFilter(5)">Cancel</el-button>
                        </div>
                    </template>
                </el-table-column>
                <el-table-column prop="campaignName" label="Campaign Name" sortable="custom">
                    <template slot="header" slot-scope="scope">
                        <label>Campaign Name</label>
                        <i class="el-icon-search search-button" v-show="!columns[6].isSearch" @click.stop="columns[6].isSearch=true"></i>
                        <div v-show="columns[6].isSearch" @click.stop="stopEvent($event)" class="input-div">
                            <el-input placeholder="Please input." size="small" v-model="columns[6].searchInput" @input="filterTableData"></el-input>
                            <el-button type="text" @click.stop="cancelFilter(6)">Cancel</el-button>
                        </div>
                    </template>
                </el-table-column>
                <el-table-column prop="campaignCode" label="campaignCode" v-if="show"></el-table-column>
                <el-table-column prop="BRANCH_CODE" label="BRANCH_CODE" v-if="show"></el-table-column>
                <el-table-column prop="SOURCE" label="SOURCE" v-if="show"></el-table-column>
                <el-table-column prop="campaignYear" label="Campaign Year" v-if="show"></el-table-column>
                <el-table-column prop="TITLE" label="TITLE" v-if="show"></el-table-column>
                <el-table-column prop="FIRST_NAME" label="FIRST_NAME" v-if="show"></el-table-column>
                <el-table-column prop="LAST_NAME" label="LAST_NAME" v-if="show"></el-table-column>
                <el-table-column prop="genderCode" label="genderCode" v-if="show"></el-table-column>
                <el-table-column prop="CITIZENSHIP" label="CITIZENSHIP" v-if="show"></el-table-column>
                <el-table-column prop="CITIZENSHIP_CODE" label="CITIZENSHIP_CODE" v-if="show"></el-table-column>
                <el-table-column prop="EMAIL" label="EMAIL" v-if="show"></el-table-column>
                <el-table-column prop="CHANNEL" label="CHANNEL" v-if="show"></el-table-column>
                <el-table-column prop="DOB" label="DOB" v-if="show" :formatter="dataFormat"></el-table-column>
                <el-table-column prop="ID_TYPE" label="ID_TYPE" v-if="show"></el-table-column>
                <el-table-column prop="ID_NUMBER" label="ID_NUMBER" v-if="show"></el-table-column>
                <el-table-column prop="COUNTRY_CODE_MOBILE" label="COUNTRY_CODE_MOBILE" v-if="show"></el-table-column>
                <el-table-column prop="COUNTRY_CODE_RESIDENT" label="COUNTRY_CODE_RESIDENT"
                                 v-if="show"></el-table-column>
                <el-table-column prop="COUNTRY_CODE_OFFICE" label="COUNTRY_CODE_OFFICE" v-if="show"></el-table-column>
                <el-table-column prop="MOBILE" label="MOBILE" v-if="show"></el-table-column>
                <el-table-column prop="RESIDENT" label="RESIDENT" v-if="show"></el-table-column>
                <el-table-column prop="OFFICE" label="OFFICE" v-if="show"></el-table-column>
                <el-table-column prop="HOME_ADDRESS" label="HOME_ADDRESS" v-if="show"></el-table-column>
                <el-table-column prop="MAILING_ADDRESS" label="MAILING_ADDRESS" v-if="show"></el-table-column>
                <el-table-column prop="LEAD_LOCATION" label="LEAD_LOCATION" v-if="show"></el-table-column>
                <el-table-column prop="STATUS" label="STATUS" v-if="show"></el-table-column>
                <el-table-column prop="LeadStatus" label="LeadStatus" v-if="show"></el-table-column>
                <el-table-column prop="PRIORITY" label="PRIORITY" v-if="show"></el-table-column>
                <el-table-column prop="PRIORITY_CODE" label="PRIORITY_CODE" v-if="show"></el-table-column>
                <el-table-column prop="CLIENT_CODE" label="CLIENT_CODE" v-if="show"></el-table-column>
                <el-table-column prop="INTRODUCER_CODE" label="INTRODUCER_CODE" v-if="show"></el-table-column>
                <el-table-column prop="LEAD_ID" label="LEAD_ID" v-if="show"></el-table-column>
                <el-table-column prop="AGENT_CODE" label="AGENT_CODE" v-if="show"></el-table-column>
                <el-table-column prop="IS_DELETED" label="IS_DELETED" v-if="show"></el-table-column>
                <el-table-column prop="ALLOCATION_TYPE_CODE" label="ALLOCATION_TYPE_CODE"
                                 v-if="show"></el-table-column>
                <el-table-column prop="CREATED_BRANCH" label="CREATED_BRANCH" v-if="show"></el-table-column>
                <el-table-column prop="STATUS_CODE" label="STATUS_CODE" v-if="show"></el-table-column>
                <el-table-column prop="SOURCE_CODE" label="SOURCE_CODE" v-if="show"></el-table-column>
                <el-table-column prop="" label="Detail" width="85" align="center">
                    <template slot-scope="scope">
                        <img class="imag" src="../../assets/images/addleads/Detail.png" @click="handleEdit(scope.row)">
                    </template>
                </el-table-column>
                <el-table-column prop="" label="Delete" align="center" width="85">
                    <!-- <img class="imag" src="../../assets/images/addleads/Delete.png" 
                        v-if="scope.row.STATUS==0"
                        title="You don’t have authority to delete this lead."
                        @mouseover="customTitleStyleAdd($event)"
                        @mouseout="customTitleStyleDel($event)"
                        @mousemove="customTitleStyleMove($event)"
                    > -->
                    <template slot-scope="scope">
                        <!-- <i class="el-icon-delete" v-if="scope.row.IS_DELETED!=3"
                           data-title="You don’t have authority to delete this lead."
                           @mouseover="customTitleStyleAdd($event)"
                           @mouseout="customTitleStyleDel($event)"
                           @mousemove="customTitleStyleMove($event)"
                           style="color:gray">
                        </i>
                        <i class="el-icon-delete" v-if="scope.row.ALLOCATION_TYPE == 'Owned'" data-title="Authorize to delete"
                           @mouseover="customTitleStyleAdd($event)"
                           @mouseout="customTitleStyleDel($event)"
                           @mousemove="customTitleStyleMove($event)"
                           @click="DelLeadsInfo(scope.$index,scope.row)"
                           style="color:gray"></i> -->
                        <span class="iconfont trash"  v-if="scope.row.ALLOCATION_TYPE=='Owned'"
                        @click="showDeleteDialog(scope.row)"></span>

                        <span class="iconfont trash"  v-else-if="scope.row.IS_DELETED==3"
                        @click="showDeleteDialog(scope.row)"></span>

                        <span class="disabled iconfont trash"  v-else data-title="You don’t have authority to delete this lead."
                        @mouseover="customTitleStyleAdd($event)"
                        @mouseout="customTitleStyleDel($event)"
                        @mousemove="customTitleStyleMove($event)"
                        style="color: rgba(0, 0, 0, 0.3); "></span> 
                    </template>
                </el-table-column>
            </el-table>
        </div>
        <div>
            <div style="text-align:center;">
                <!-- <el-pagination
                        :total="tableData.length"
                        :current-page.sync="curPage"
                        :page-size="pageSize"
                        :page-sizes="[8,16,24,32,40,48]"
                        :page-count="7"
                        background
                        @current-change="changePage"
                        @size-change="changeSize"
                        layout="prev, pager, next,sizes">
                </el-pagination> -->
                <gl-page :total="tableData.length" :current-page.sync="curPage" @current-change="changePage" @size-change="changeSize">
                </gl-page>
            </div>
            <div class="bottom-fixed-button" style="position: relative">
                <outTable name="Allocated Leads" :tableData="this.tableExport"/>
            </div>
        </div>
        <AddLeads ref="addLeads" :flag="this.flag" :campaginYearflag="this.campaginYearflag"
                  :campaginNameflag="this.campaginNameflag" :formBeforeEdit="this.formBeforeEdit"></AddLeads>
        <Errors ref="errors"></Errors>
        <InfoDialog @leftButtonFunction="closeDialog" @rightButtonFunction="DelLeadsInfo(deleteRow)" v-show="dialogVisibility">
            <div class="slot-info">Are you sure to delete this lead?</div>
        </InfoDialog>
    </div>
</template>
<script>
    import * as fecha from 'element-ui/lib/utils/date';
    import outTable from "../../pages/tab/Export.vue";
    import Errors from "../../components/common/Errors";
    import $ from "jquery";
    import Select from "../../components/element/select/select";
    import AddLeads from "./AddLeads";
    import InfoDialog from '../../components/common/InfoDialog'

    import '../../assets/iconfont/iconfont.css'


    export default {
        props: ['STATUS'],
        data() {
            return {
                formBeforeEdit: {}, 
                dialogVisibility: false,
                flag: false,
                campaginYearflag: false,
                campaginNameflag: false,
                baseUrl: domain.baseUrl,
                show: false,
                upd_flag: "1",
                depss: [],
                depatment: '',
                filterGender: [
                    {text: 'Male', value: 'Male'},
                    {text: 'Female', value: 'Female'}
                ],
                filterAllocationType: [
                    {text: 'Assigned', value: 'Assigned'},
                    {text: 'Owned', value: 'Owned'}
                ],
                pageSize: 8,
                curPage: 1,
                tableData: [],
                tableExport: [],
                columns: [
                    {field: 'Name', title: 'Lead Name',isSearch:false,searchInput:null},
                    {field: 'Phone', title: 'Contact No.Mobile',isSearch:false,searchInput:null},
                    {field: 'Gender', title: 'Gender',isSearch:false,searchInput:null},
                    {field: 'LEADS_CALL_DATE', title: 'Leads Call Date',isSearch:false,searchInput:null},
                    {field: 'UPDATE_DATE', title: 'Last Update Date',isSearch:false,searchInput:null},
                    {field: 'ALLOCATION_TYPE', title: 'Allocation Type',isSearch:false,searchInput:null},
                    {field: 'campaignName', title: 'Campaign Name',isSearch:false,searchInput:null}
                ],
                tableDataAgent: [],
                form: {
                    LAN_ID: '',
                    DateFrom: '',
                    DateTo: ''
                },
                ruleForm: {
                    genderCode: '',
                    titleCode: '',
                    CurrentYear: '',
                    compaignYear: '',
                    campaignName: '',
                    Title: '',
                    FirstName: '',
                    LastName: '',
                    Gender: '',
                    Citizenship: '',
                    Email: '',
                    Channel: '',
                    Branch: '',
                    AllocationType: '',
                    DateBirth: '',
                    IDENTITY_TYPE: '',
                    IDENTITY_NO: '',
                    LeadsCallDate: '',
                    CountryCodeMobile: '',
                    CountryCodeResident: '',
                    CountryCodeOffice: '',
                    Mobile: '',
                    Resident: '',
                    Office: '',
                    HomeAddress: '',
                    MailingAddress: '',
                    LeadLocation: '',
                    LeadStatus: '',
                    Priority: '',
                    ClientCode: '',
                    IntroducerCode: '',
                    AgentCode: '',
                    BRANCHLAST: '',
                    SOURCE: '',
                    campaignCode: '',
                    LEAD_ID: ''
                },
                statusValue: '',
                copyTableData:[],
                filters:{},
            }
        },
        created() {
            this.convert();
        },
        mounted() {
        },
        components: {
            Select,
            outTable,
            Errors,
            AddLeads,
            InfoDialog
        },
        computed:{
            calculateTableData(){
                // this.filterTableData();
                return this.tableData.slice((this.curPage-1)*this.pageSize,this.curPage*this.pageSize);
            }
        },
        methods: {
            showDeleteDialog(row) {
                this.dialogVisibility = true;
                this.deleteRow = row;
            },
            closeDialog() {
                this.dialogVisibility = false;
            },
            sortFieldChange(table){
                let methodName='sort'+table.prop;
                try{
                    console.log('mehodName:',table,methodName);
                    this[methodName](table.order);
                }catch(e){
                    console.log(e);
                }
            },
            sortName(order){
                this.tableData = this.tableData.sort(
                    function(a, b) {
                        return order == 'ascending' ? a.Name.localeCompare(b.Name) : b.Name.localeCompare(a.Name)
                    }
                )
            },
            sortPhone(order){
                this.tableData=this.tableData.sort(
                    function(a,b){
                        let aCode = a.Phone.slice(1, a.Phone.indexOf(')'));
                        let bCode = b.Phone.slice(1, b.Phone.indexOf(')'));
                        if(aCode != bCode) {
                            return order=='ascending'?aCode-bCode:bCode-aCode;
                        } else {
                            let aNumber = a.Phone.slice(a.Phone.indexOf(')')+1);
                            let bNumber = b.Phone.slice(b.Phone.indexOf(')')+1);
                            let arrayA = aNumber.split('');
                            let arrayB = bNumber.split('');
                            let minLength = Math.min(arrayA.length,arrayB.length);
                            for(var i = 0; i < minLength; i++){
                                let currA = arrayA[i] - 0;
                                let currB = arrayB[i] - 0;
                                let currSub = currA - currB;  
                                if (currSub != 0)
                                {
                                    return order === "ascending" ? currA - currB : currB - currA;
                                }      
                            }
                            return order === "ascending" ? aNumber.length - bNumber.length : bNumber.length - aNumber.length; 
                        }
                    }
                );
            },
            sortGender(order){               
                this.tableData=this.tableData.sort(function(a,b){return order=='ascending'?a.Gender.localeCompare(b.Gender):b.Gender.localeCompare(a.Gender);});
            },
            sortALLOCATION_TYPE(order){
                this.tableData.sort(
                    function(a,b){
                        return order=='ascending'?a.ALLOCATION_TYPE.localeCompare(b.ALLOCATION_TYPE):b.ALLOCATION_TYPE.localeCompare(a.ALLOCATION_TYPE);
                    }
                );
            },
            sortcampaignName(order){
                this.mixSort(order, 'campaignName')
                // this.tableData.sort(
                //     function(a,b){
                //         let aCN=a.campaignName?a.campaignName:'-Infinity';
                //         let bCN=b.campaignName?b.campaignName:'-Infinity';

                //         return order=='ascending'?aCN.localeCompare(bCN):bCN.localeCompare(aCN);
                //     }
                // );
            },

            // mixSort:function(order,field)
            // {
            //     this.tableData = this.tableData.sort(
            //         function(a,b)
            //         {
            //             let aName,bName;
            //             if(order == 'ascending') {
            //                 aName = a[field] ? a[field] : 'zzzzzzzzzz';
            //                 bName = b[field] ? b[field] : 'zzzzzzzzzz';
            //             } else {
            //                 aName = a[field] ? a[field] : '0000000000';
            //                 bName = b[field] ? b[field] : '0000000000';
            //             }
            //             let isSort = -1;
            //             if (/^1653\d/.test(aName) ^ /^\D/.test(bName))
            //             {
            //                 isSort = (aName > bName ?1:(aName == bName ?0:-1));
            //             }else
            //             {
            //                 isSort = (aName < bName ?-1:(aName == bName ?0:1));
            //             }
            //             if (order === 'ascending')
            //             {
            //                 return isSort;
            //             }else
            //             {
            //                 return -1 * isSort;
            //             }
            //         }
            //     );
            // },

            mixSort:function(order,field)
            {
                this.tableData = this.tableData.sort(
                    function(a,b)
                    {
                        let aName,bName;
                        if(order == 'ascending') {
                            aName = a[field] ? a[field] : 'ZZZZZZZZ';
                            bName = b[field] ? b[field] : 'ZZZZZZZZ';
                        } else {
                            aName = a[field] ? a[field] : '00000000';
                            bName = b[field] ? b[field] : '00000000';
                        }
                        let aArray = aName.split('');
                        let bArray = bName.split('');
                        let min = Math.min(aArray.length, bArray.length)
                        console.log(min);
                        for(let i = 0; i < min; i ++){
                            if(aArray[i] != bArray[i]) {
                                if(/^[0-9]*$/.test(aArray[i])  && /^[0-9]*$/.test(bArray[i])){
                                    return order == 'ascending' ? aArray[i] - bArray[i] : bArray[i] - aArray[i]
                                }
                                
                                if(/^[a-zA-Z]*$/.test(aArray[i])  && /^[a-zA-Z]*$/.test(bArray[i])) {
                                    return order == 'ascending' ? aArray[i].localeCompare(bArray[i]) : bArray[i].localeCompare(aArray[i])
                                }

                                if(/^[0-9]*$/.test(aArray[i]) &&  /^[a-zA-Z]*$/.test(bArray[i])) {
                                    return order == 'ascending' ? -1 : 1
                                }

                                if(/^[a-zA-Z]*$/.test(aArray[i]) && /^[0-9]*$/.test(bArray[i])) {
                                    return order == 'ascending' ? 1 : -1
                                }
                            }
                        }
                        return order == 'ascending' ? aArray.length - bArray.length : bArray.length - aArray.length
                    }
                );
            },
            
            sortDateString(order,field){
               this.tableData.sort(
                    function(a,b){
                        let aTime, bTime;
                        if(order == 'ascending') {
                            aTime = a[field] ? (new Date(a[field])).getTime():+Infinity;
                            bTime = b[field] ? (new Date(b[field])).getTime():+Infinity;
                        } else {
                            aTime = a[field] ? (new Date(a[field])).getTime():-Infinity;
                            bTime = b[field] ? (new Date(b[field])).getTime():-Infinity;
                        }
                        return order=='ascending'?aTime-bTime:bTime-aTime;
                    }
                );
            },
            sortLEADS_CALL_DATE(order){
                this.sortDateString(order, 'LEADS_CALL_DATE')
                // this.tableData.sort(function(a,b)
                // {
                //     let aTime = 0;
                //     let bTime = 0;
                //     let field = 'LEADS_CALL_DATE';
                //     if (!$.isEmptyObject(b[field]))
                //     {
                //         if (order === 'ascending')
                //         {
                //             bTime = b[field] ? (new Date(b[field])).getTime():+Infinity;
                //         }else
                //         {
                //             bTime = b[field] ? (new Date(b[field])).getTime():-Infinity;
                //         }
                        
                //     }
                //     if (!$.isEmptyObject(a[field]))
                //     {
                //         if (order === 'ascending')
                //         {
                //             aTime = a[field] ? (new Date(a[field])).getTime():+Infinity;
                //         }else
                //         {
                //             aTime = a[field] ? (new Date(a[field])).getTime():-Infinity;
                //         }
                        
                //     }
                //     return order === 'ascending' ? aTime - bTime : bTime - aTime;
                // });
            },
            sortUPDATE_DATE(order){
                this.sortDateString(order,'UPDATE_DATE');
            },
            customTitleStyleAdd(e) {
                var x = -100;// -210;
                var y = 10;//-160;
                this.mytitle = e.target.getAttribute("data-title");
                var tooltip = "<div id='tplink'>" + this.mytitle + "</div>";//create div element
                $("body").append(tooltip);
                $("#tplink").css({"top": (e.pageY + y) + "px", "left": (e.pageX + x) + "px"});
                $("#tplink").css({"position": "absolute", "color": "#fff", "max-width": "360px"});
                $("#tplink").css({"font-size": "14px", "padding": "8px 12px", "border": "1px solid rgb(233,247,246)"});
                $("#tplink").css({"border-radius": "5px", "margin": "5px 10px"});
                $("#tplink").css({"background-color": "#D31145", "z-index": 1});
                $("#tplink").show();
            },
            customTitleStyleDel(e) {
                $("#tplink").remove();
            },
            customTitleStyleMove(e) {
                var x = -100;// -210;
                var y = 10;//-160;
                $("#tplink").css({"top": (e.pageY + y) + "px", "left": (e.pageX + x) + "px"});
            },
            convert() {
                var arr = this;
                var param = {
                    leadStatus: this.STATUS,
                    agentCode: JSON.parse(localStorage.getItem("thisAgentCode"))
                };
                var Mobile = "";
                // var gender = "";
                var leadsByList = [];
                this.$axios({
                    method: 'post',
                    url: this.baseUrl + 'lead/getLeadsListByStatus/',
                    contentType: "application/json;charset=utf-8",
                    data: param
                }).then(function (response) {
                    console.log(response.data.DATA);
                    if (response.data.RESPONSE_CD == "0") {
                        for (var i = 0; i < response.data.DATA.leadsList.length; i++) {
                            var Name = response.data.DATA.leadsList[i].leadName;
                            Mobile = "(" + response.data.DATA.leadsList[i].COUNTRY_CODE_MOBILE + ")" +
                                response.data.DATA.leadsList[i].MOBILE;
                            var Gender;
                            if (response.data.DATA.leadsList[i].GENDER === "male") {
                                Gender = "Male";
                            } else {
                                Gender = "Female";
                            }
                            arr.tableExport.push(
                                {   
                                    "CAMPAIGN YEAR": response.data.DATA.leadsList[i].campaignYear,
                                    "CAMPAIGN NAME": response.data.DATA.leadsList[i].campaignName,
                                    "ID NUMBER": response.data.DATA.leadsList[i].ID_NUMBER,
                                    "ID TYPE": response.data.DATA.leadsList[i].ID_TYPE,
                                    "LEAD NAME":Name,
                                    "GENDER": Gender,
                                    "CITIZENSHIP": response.data.DATA.leadsList[i].CITIZENSHIP,
                                    "EMAIL":response.data.DATA.leadsList[i].EMAIL,
                                    "CHANNEL": response.data.DATA.leadsList[i].CHANNEL,
                                    "BRANCH": response.data.DATA.leadsList[i].BRANCH_CODE,
                                    "AGENT CODE": response.data.DATA.leadsList[i].AGENT_CODE,
                                    "DOB": response.data.DATA.leadsList[i].DOB,
                                    "PRIORITY": response.data.DATA.leadsList[i].PRIORITY,
                                    "CLIENT CODE": response.data.DATA.leadsList[i].CLIENT_CODE,
                                    "INTRODUCER CODE": response.data.DATA.leadsList[i].INTRODUCER_CODE,
                                    "LEADS CALL DATE": response.data.DATA.leadsList[i].LEADS_CALL_DATE,
                                    "COUNTRY CODE FOR MOBILE": response.data.DATA.leadsList[i].COUNTRY_CODE_MOBILE,
                                    "CONTACT NO. MOBILE": response.data.DATA.leadsList[i].MOBILE,
                                    "COUNTRY CODE FOR RESIDENT": response.data.DATA.leadsList[i].COUNTRY_CODE_RESIDENT,
                                    "CONTACT NO. RESIDENT": response.data.DATA.leadsList[i].RESIDENT,
                                    "COUNTRY CODE FOR OFFICE": response.data.DATA.leadsList[i].COUNTRY_CODE_OFFICE,
                                    "CONTACT NO. OFFICE": response.data.DATA.leadsList[i].OFFICE,
                                    "HOME ADDRESS": response.data.DATA.leadsList[i].HOME_ADDRESS,
                                    "MAILING ADDRESS": response.data.DATA.leadsList[i].MAILING_ADDRESS,
                                    "LEAD LOCATION": response.data.DATA.leadsList[i].LEAD_LOCATION,
                                    "LEAD STATUS": response.data.DATA.leadsList[i].STATUS,
                                    "ALLOCATION TYPE": response.data.DATA.leadsList[i].ALLOCATION_TYPE,
                                    "SOURCE TYPE": response.data.DATA.leadsList[i].SOURCE,
                                    "BANK NAME": response.data.DATA.leadsList[i].BANK_NAME,
                                    "POSTAL CODE": response.data.DATA.leadsList[i].POSTAL_ADDRESS,
                                    "POLICY NUMBER&DETAILS": response.data.DATA.leadsList[i].POLICY_NUMBER,
                                    "AMOUNT": response.data.DATA.leadsList[i].AMOUNT,
                                    "COMPLETE POSSIBILITY": response.data.DATA.leadsList[i].COMPLETE_POSSIBILITY,
                                    "POSTPONED/FOLLOW UP DATE": response.data.DATA.leadsList[i].POSTPONED_FOLLOW_UP_DATE,
                                }
                            );
                            leadsByList.push(
                                {
                                    
                                    "Phone": Mobile,
                                    "campaignYear": response.data.DATA.leadsList[i].campaignYear,
                                    "campaignCode": response.data.DATA.leadsList[i].campaignCode,
                                    "campaignName": response.data.DATA.leadsList[i].campaignName,
                                    "Name": response.data.DATA.leadsList[i].leadName,
                                    "Gender": response.data.DATA.leadsList[i].GENDER,
                                    "genderCode": response.data.DATA.leadsList[i].GENDER_CODE,
                                    "LEADS_CALL_DATE": response.data.DATA.leadsList[i].LEADS_CALL_DATE,
                                    "UPDATE_DATE": response.data.DATA.leadsList[i].UPDATE_DATE,
                                    "ALLOCATION_TYPE": response.data.DATA.leadsList[i].ALLOCATION_TYPE,
                                    "ALLOCATION_TYPE_CODE": response.data.DATA.leadsList[i].ALLOCATION_TYPE_CODE,
                                    "AGENT_CODE": response.data.DATA.leadsList[i].AGENT_CODE,
                                    "ID_NUMBER": response.data.DATA.leadsList[i].ID_NUMBER,
                                    "COUNTRY_CODE_MOBILE": response.data.DATA.leadsList[i].COUNTRY_CODE_MOBILE,
                                    "PRIORITY": response.data.DATA.leadsList[i].PRIORITY,
                                    "PRIORITY_CODE": response.data.DATA.leadsList[i].PRIORITY_CODE,
                                    "ID_TYPE": response.data.DATA.leadsList[i].ID_TYPE,
                                    "ID_TYPE_CODE": response.data.DATA.leadsList[i].ID_TYPE_CODE,

                                    "CREATED_BRANCH": response.data.DATA.leadsList[i].CREATED_BRANCH,
                                    "LAST_NAME": response.data.DATA.leadsList[i].LAST_NAME,
                                    "EMAIL": response.data.DATA.leadsList[i].EMAIL,
                                    "LEAD_LOCATION": response.data.DATA.leadsList[i].LEAD_LOCATION,
                                    "FIRST_NAME": response.data.DATA.leadsList[i].FIRST_NAME,
                                    "COUNTRY_CODE_OFFICE": response.data.DATA.leadsList[i].COUNTRY_CODE_OFFICE,
                                    "LEAD_ID": response.data.DATA.leadsList[i].LEAD_ID,
                                    "STATUS": response.data.DATA.leadsList[i].STATUS,
                                    "LeadStatus": response.data.DATA.leadsList[i].LEAD_STATUS,
                                    "CITIZENSHIP": response.data.DATA.leadsList[i].CITIZENSHIP,
                                    "CITIZENSHIP_CODE": response.data.DATA.leadsList[i].CITIZENSHIP_CODE,
                                    "COUNTRY_CODE_RESIDENT": response.data.DATA.leadsList[i].COUNTRY_CODE_RESIDENT,
                                    "CLIENT_CODE": response.data.DATA.leadsList[i].CLIENT_CODE,
                                    "DOB": response.data.DATA.leadsList[i].DOB,

                                    "BRANCH": response.data.DATA.leadsList[i].BRANCH,
                                    "BRANCH_CODE": response.data.DATA.leadsList[i].BRANCH_CODE,
                                    "RESIDENT": response.data.DATA.leadsList[i].RESIDENT,
                                    "INTRODUCER_CODE": response.data.DATA.leadsList[i].INTRODUCER_CODE,

                                    "MAILING_ADDRESS": response.data.DATA.leadsList[i].MAILING_ADDRESS,
                                    "HOME_ADDRESS": response.data.DATA.leadsList[i].HOME_ADDRESS,
                                    "CHANNEL": response.data.DATA.leadsList[i].CHANNEL,
                                    "OFFICE": response.data.DATA.leadsList[i].OFFICE,
                                    "SOURCE": response.data.DATA.leadsList[i].SOURCE,
                                    "SOURCE_CODE": response.data.DATA.leadsList[i].SOURCE_CODE,
                                    "TITLE": response.data.DATA.leadsList[i].TITLE,
                                    "IS_DELETED": response.data.DATA.leadsList[i].IS_DELETED,

                                    "MOBILE": response.data.DATA.leadsList[i].MOBILE,
                                    "AMOUNT": response.data.DATA.leadsList[i].AMOUNT,
                                    "COMPLETE_POSSIBILITY": response.data.DATA.leadsList[i].COMPLETE_POSSIBILITY,
                                    "BANK_NAME": response.data.DATA.leadsList[i].BANK_NAME,
                                    "POSTAL_ADDRESS": response.data.DATA.leadsList[i].POSTAL_ADDRESS,
                                    "POLICY_NUMBER": response.data.DATA.leadsList[i].POLICY_NUMBER,
                                    "POSTPONED_FOLLOW_UP_DATE": response.data.DATA.leadsList[i].POSTPONED_FOLLOW_UP_DATE,
                                    "isDisplay":false,
                                    "expire_date": response.data.DATA.leadsList[i].EXPIRED_DATE
                                }
                            );
                        }
                        arr.tableData = leadsByList;
                        arr.copyTableData=leadsByList;
                    } else {
                        // this.$refs&&this.$refs.errors.showLoadDataError("123");
                        return;
                    }
                    
                    arr.sortLEADS_CALL_DATE('ascending', 'LEADS_CALL_DATE')
                }).catch(function (error) {
                    console.log(error);
                });
            },
            getValue: function (value) {
                console.log(value);
            },
            AddLeadsInfo: function () {
                this.$axios({
                    method: 'POST',
                    url: this.baseUrl + 'config/getconfigInfo',
                    contentType: "application/json;charset=utf-8",
                    data: {}
                }).then(response => {
                    if (response.data.RESPONSE_CD == "0") {
                        let leadsCallDate = response.data.DATA.leadsCallDate;
                        let callDate = new Date().getTime() + leadsCallDate * 24 * 60 * 60 * 1000;
                        this.$refs.addLeads.ruleForm.LeadsCallDate = (new Date(callDate).getMonth()+1)+'/'+ new Date(callDate).getDate()+'/'+new Date(callDate).getFullYear(); 
                    }
                })

                this.flag = true;
                this.campaginYearflag = false;
                this.campaginNameflag = false;
                this.$refs.addLeads.upd_flag = "1";
                // this.$refs.addLeads.ruleForm.CurrentYear = this.$refs.addLeads.doHandleYear();
                this.$refs.addLeads.getYearList();
                this.$refs.addLeads.getBranchAgentCode();
                this.$refs.addLeads.getAllOptionList();
                this.$refs.addLeads.getCountryCodeMobileList();
                //this.$refs.addLeads.getAuditTrailInfo();
                this.statusValue = "";
                this.$refs.errors.createCustomMask();
                document.getElementById("addLeadsInfo").style.display = "block";
            },
            DelLeadsInfo: function (row) {
                // debugger;
                var param = {
                    "LEAD_ID": row.LEAD_ID,
                    THIS_AGENT_CODE: JSON.parse(localStorage.getItem("thisAgentCode"))
                    };
                this.$axios({
                    method: "post",
                    url: this.baseUrl + 'lead/delete',
                    data: param
                }).then((response) => {
                    if (response.data.RESPONSE_CD == "0") {
                        //alert("Delete Successful !");
                        this.tableData = [];
                        this.tableExport = [];
                        this.convert();
                        this.$message({
                            message: 'Delete successfully.',
                            type: 'success'
                        })
                        this.closeDialog();
                        setTimeout(() => {
                            document.location.reload();
                        }, 2000)
                    } else {
                        // this.$refs.errors.showLoadDataError(response.data.RESPONSE_DESC);
                        return false;
                    }
                }).catch((error) => {
                    console.log(error);
                    return false;
                });
            },
            handleEdit(row, id) {
                if (row.ALLOCATION_TYPE == "Owned") {
			        this.GLOBAL.leadsCallDateflag = false;
                } else { 
                   this.GLOBAL.leadsCallDateflag = true;
                }
		        if (row.LeadStatus == "5") {
                  this.GLOBAL.Amountflag = false;
                  this.GLOBAL.completePossibilityflag = false;
                } else {
                  this.GLOBAL.Amountflag = true;
                  this.GLOBAL.completePossibilityflag = true;
                }

                this.campaginYearflag = true;
                this.campaginNameflag = true;
                this.$refs.addLeads.postponeWarningFlag = false;
                this.$refs.addLeads.PostponeFlag = true;
                console.log(row.LeadStatus);
                if(row.LeadStatus == '7' || row.LeadStatus == '6'){
                    console.log('in');
                    this.$refs.addLeads.PostponeFlag = false;
                }
                this.$refs.addLeads.getYearList();
                this.$refs.addLeads.getBranchAgentCode();
                this.$refs.addLeads.getAllOptionList();
                this.$refs.addLeads.getCountryCodeMobileList();
                this.$refs.addLeads.getAuditTrailInfo(row.LEAD_ID);
                this.$refs.errors.createCustomMask();
                document.getElementById("addLeadsInfo").style.display = "block";
                this.$refs.addLeads.upd_flag = "0";
                this.$refs.addLeads.ruleForm.CurrentYear = row.campaignYear;
                this.$refs.addLeads.ruleForm.CampaignNameSelected = row.campaignName;
                this.$refs.addLeads.ruleForm.titleCode = row.TITLE;
                this.$refs.addLeads.ruleForm.FirstName = row.FIRST_NAME;
                this.$refs.addLeads.ruleForm.LastName = row.LAST_NAME;
                this.$refs.addLeads.ruleForm.Gender = row.Gender;
                this.$refs.addLeads.ruleForm.genderCode = row.genderCode;
                this.$refs.addLeads.ruleForm.Citizenship = row.CITIZENSHIP;
                this.$refs.addLeads.ruleForm.CitizenshipCode = row.CITIZENSHIP_CODE;
                this.$refs.addLeads.ruleForm.Email = row.EMAIL;
                this.$refs.addLeads.ruleForm.Channel = row.CHANNEL;
                this.$refs.addLeads.ruleForm.Branch = row.BRANCH;
                this.$refs.addLeads.ruleForm.BranchCode = row.BRANCH_CODE;
                this.$refs.addLeads.ruleForm.AllocationType = row.ALLOCATION_TYPE;
                this.$refs.addLeads.ruleForm.AllocationTypeCode = row.ALLOCATION_TYPE_CODE;
                this.$refs.addLeads.ruleForm.DateBirth = row.DOB;
                this.$refs.addLeads.ruleForm.IDENTITY_TYPE = row.ID_TYPE;
                this.$refs.addLeads.ruleForm.IDENTITY_TYPE_CODE = row.ID_TYPE_CODE;
                this.$refs.addLeads.ruleForm.IDENTITY_NO = row.ID_NUMBER;
                this.$refs.addLeads.ruleForm.LeadsCallDate = row.LEADS_CALL_DATE;
                this.$refs.addLeads.ruleForm.CountryCodeMobile = row.COUNTRY_CODE_MOBILE == '' ? '' : "+" + row.COUNTRY_CODE_MOBILE;
                this.$refs.addLeads.ruleForm.CountryCodeResident = row.COUNTRY_CODE_RESIDENT ? (row.COUNTRY_CODE_RESIDENT == '' ? '' : "+" + row.COUNTRY_CODE_RESIDENT) : ''; 
                this.$refs.addLeads.ruleForm.CountryCodeOffice = row.COUNTRY_CODE_OFFICE ? (row.COUNTRY_CODE_OFFICE == '' ? '' : "+" + row.COUNTRY_CODE_OFFICE) : '';
                this.$refs.addLeads.ruleForm.Mobile = row.MOBILE;
                this.$refs.addLeads.ruleForm.Resident = row.RESIDENT;
                this.$refs.addLeads.ruleForm.Office = row.OFFICE;
                this.$refs.addLeads.ruleForm.Status = row.STATUS;
                this.$refs.addLeads.ruleForm.LeadStatus = row.LeadStatus;
                this.$refs.addLeads.ruleForm.HomeAddress = row.HOME_ADDRESS;
                this.$refs.addLeads.ruleForm.MailingAddress = row.MAILING_ADDRESS;
                this.$refs.addLeads.ruleForm.LeadLocation = row.LEAD_LOCATION;
                this.$refs.addLeads.ruleForm.Priority = row.PRIORITY;
                this.$refs.addLeads.ruleForm.PriorityCode = row.PRIORITY_CODE;
                this.$refs.addLeads.ruleForm.ClientCode = row.CLIENT_CODE;
                this.$refs.addLeads.ruleForm.IntroducerCode = row.INTRODUCER_CODE;
                this.$refs.addLeads.ruleForm.AgentCode = row.AGENT_CODE;

                this.$refs.addLeads.ruleForm.BRANCHLAST = row.BRANCH;
                this.$refs.addLeads.ruleForm.SourceType = row.SOURCE;
                this.$refs.addLeads.ruleForm.SourceTypeCode = row.SOURCE_CODE;
                this.$refs.addLeads.ruleForm.campaignCode = row.campaignCode;
                this.$refs.addLeads.ruleForm.LEAD_ID = row.LEAD_ID;
                this.$refs.addLeads.ruleForm.LockStatus = row.IS_DELETED;
                this.$refs.addLeads.ruleForm.Amount = row.AMOUNT;
                this.$refs.addLeads.ruleForm.completePossibility = row.COMPLETE_POSSIBILITY;
                this.$refs.addLeads.ruleForm.BankName = row.BANK_NAME;
                this.$refs.addLeads.ruleForm.PostalAddress = row.POSTAL_ADDRESS;
                this.$refs.addLeads.ruleForm.PolicyNumber = row.POLICY_NUMBER;
                this.$refs.addLeads.ruleForm.PostponedFollowUpDate = row.POSTPONED_FOLLOW_UP_DATE;
                this.$refs.addLeads.ruleForm.expire_date = row.expire_date;
                    
                for(let i in this.$refs.addLeads.ruleForm) {
                    this.formBeforeEdit[i] = this.$refs.addLeads.ruleForm[i]
                }
            },

            tableRowStyle({row, rowIndex}) {
                return 'width:1391px;height:46px;border:1px solid rgba(219,218,212,1);font-size:14px;font-family:"AIAEverest" ,"Regular";' +
                    'color:rgba(85,67,68,1);line-height:18px;text-align:center;'
            },

            tableHeaderColor({row, column, rowIndex, columnIndex}) {
                return 'font-size:15px;font-family:AIAEverestCondensedMedium;' +
                    'background:rgba(242,245,247,1);border-radius:4px 4px 0px 0px;color:rgba(85,67,68,1);'
            },

            setFilters(filters){
                this.filters=filters;
                this.filterTableData();
            },

            filterFieldChange(tempData){
                if(Object.keys(this.filters).length!=0){
                    let temp=[];
                    for(let prop in this.filters){
                        this.filters[prop].forEach(x=>{
                            temp=tempData.filter(data=>data[prop].toLowerCase() === x.toLowerCase());
                            this.tableData=temp;
                        })                            
                    }
                }
            },
            filterTableData(){
                let hasSearch=this.columns.some(column=>column.isSearch||Object.keys(this.filters).length!=0);

                if(hasSearch){
                    this.tableData=this.copyTableData;
                    this.columns.forEach(x=>{
                        if(x.isSearch){
                            let temp=this.tableData;
                            if(x.searchInput){                            
                                temp=this.tableData.filter(item=>
                                item[x.field] && item[x.field].toLowerCase().search(x.searchInput.toLowerCase())>=0);
                                this.tableData=temp;                     
                            }
                        }
                    }); 
                    
                    this.filterFieldChange(this.tableData);
                }else{
                    this.tableData=this.copyTableData;
                }
            },
            
            cancelFilter(index){
                this.columns[index].searchInput=null;
                this.columns[index].isSearch=false;
                this.filterTableData();
            },
            stopEvent(e){e.stopPropagation();},
            dataFormat(row, column, cellValue) {
                //  return cellValue ? fecha.format(new Date(cellValue), 'dd/MM/yyyy') : '';
                return cellValue;
            },
            leadsCallDataFormat(row, column, cellValue) {
                //  return cellValue ? fecha.format(new Date(cellValue), 'dd/MM/yyyy') : '';
                
                if(cellValue == undefined) return;
                return cellValue.slice(0,10);
            },

            handleSizeChange(val) {
                this.pageSize = val;
            },

            handleCurrentChange(val) {
                this.currentPage = val;

            },
            async changePage(page) {
                this.curPag=page;
            },
            changeSize(size) {
                this.pageSize = size;
            },
        }
    }
</script>


<style lang="scss" scoped>
    .bottom-page {
        width: 100%;
        height: 100%;
        background: rgba(255, 255, 255, 1);
        margin-top: 0px;

    }

    .table-list {
        // margin-left: 30px;
        // text-align: center;
        // width: 95%;
        // heigth: 420px;
        // overflow: auto;
        // border-radius: 4px;
        border: 1px solid rgba(219, 218, 212, 1);
        margin-bottom:24px;
    }

    // .font-s {
    //     font: normal 16px "AIASans", "Calibri", sans-serif;
    //     text-align: center;
    //     font-weight: bold;
    // }


    // .inputStyle {
    //     width: 401px !important;
    //     border-radius: 0px;

    // }

    // .inputStyle01 {

    //     width: 150px !important;
    //     border-radius: 0px;
    //     border: 0px solid #A9A9A9;
    // }

    // .pickerStyle {

    //     width: 187px !important;
    //     border-radius: 0px;


    // }


    // .inputStylePop {
    //     width: 120px !important;
    //     border-radius: 0px;
    // }

    // .inputStyleEmail {

    //     width: 300px !important;
    //     border-radius: 0px;

    // }

    // .inputStyleNumber {

    //     width: 210px !important;
    //     border-radius: 0px;

    // }

    // .inputStyleDOB {
    //     width: 210px !important;
    //     border-radius: 0px;

    // }

    // .inputStyleType {
    //     width: 150px !important;
    //     border-radius: 0px;

    // }

    // .inputStyleNo {
    //     width: 201px !important;
    //     border-radius: 0px;

    // }

    // .inputStyleCode {
    //     width: 80px !important;
    //     border-radius: 0px;


    // }

    // .inputStyleMobile {
    //     width: 180px !important;
    //     border-radius: 0px;

    // }

    // .inputStyleAddress {
    //     //width:853px!important;
    //     border-radius: 0px;


    // }

    // .inputStyleCountryCode {
    //     width: 68px !important;
    //     border-radius: 0px;
    // }

    // .textareaStyle {
    //     min-height: 70px;
    //     height: 70px;
    // }

    #tplink {
        position: absolute;
        color: red;
        max-width: 160px;
        font-size: 14px;
        padding: 4px;
        background: rgba(40, 40, 40, 0.8);
        border: solid 1px #e9f7f6;
        border-radius: 5px;
    }
    img{
        cursor: pointer;
    }

    .el-table th>.cell{
        display:flex;
        flex-wrap: wrap;
        align-items: center;

        .search-button{
            margin-left: 4px;
            margin-top: 1px;
            color: #848484;
        }


        .el-input{
            padding:0;
            .el-input__inner{
                height:32px;
            }
        }

        .input-div{
            display: flex;
            padding: 0;
            order: 3;

            .el-button--text{
                margin-left: 5px;
            }
        }
        .caret-wrapper{
            order: 2;
            margin-top: 4px;
        }

        .el-table__column-filter-trigger{
            order:3;
        }
    }

    .modal-upload-body{
        display: flex;
        justify-content: center;
    }

    .slot-info{
        margin-top: 100px;
        font-size: 24px;
    }

    .iconfont{
        font-size: 22px!important;
        color:#596C80;
    }

    .disabled{
        color: rgba(0, 0, 0, 0.1);
    }

    /deep/ .modal-upload-body {
        display: flex;
        justify-content: center;
    }
</style>
