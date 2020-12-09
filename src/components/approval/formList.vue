<template>
    <div class="wrap_w">
        <div v-for="(item,index) in list" :key="index">
            <van-cell-group v-if="item.type=='S'||item.type=='E'||item.type=='N'||item.type=='H'" custom-class="cell">
                <van-field
                    :value="item.value"
                    custom-style="font-size:34rpx;color:#333333"
                    :required="item.require||false"
                    :label="item.label"
                    :placeholder="item.helpText"
                    input-align="right"
                    @change="function(val){changeInput(val,item)}"
                    :disabled="disabled"
                />
            </van-cell-group>
            <van-cell-group custom-class="cell" v-if="item.type=='L'||item.type=='DT'||item.type=='LT'">
                <picker :disabled="disabled" @change="(val)=>{bindPickerChange(val,item)}" :value="item.index" range-key="label" :range="currenData[item.id]">
                    <van-field
                        :value="currenData[item.id][item.index]?currenData[item.id][item.index].label:''"
                        input-class="inp"
                        custom-style="font-size:34rpx;color:#333333"
                        :required="item.require||false"
                        :disabled="disabled"
                        :label="item.label"
                        :placeholder="item.helpText"
                        input-align="right"
                        right-icon="arrow"
                    />
                </picker>
            </van-cell-group>
            <van-cell-group custom-class="cell" v-if="item.type=='D'">
                <picker :disabled="disabled" mode="date" :value="item.value" @change="function(val){bindDateChange(val,item)}">
                    <van-field
                        :value="item.value"
                        title-width="110px"
                        input-class="inp"
                        custom-style="font-size:34rpx;color:#333333"
                        :required="item.require||false"
                        :disabled="disabled"
                        :label="item.label"
                        input-align="right"
                        :placeholder="item.helpText"
                    />
                </picker>
            </van-cell-group>
            <van-cell-group custom-class="cell" v-if="item.type=='F'">
                <picker :disabled="disabled" class="picker" mode="multiSelector" :value="item.multiIndex" @change="((val)=>{bindMultiPickerChange(val,item)})"
                    :range="newMultiArrayList">
                    <van-field
                        :value="item.value"
                        input-class="inp"
                        custom-style="font-size:34rpx;color:#333333"
                        :required="item.require||false"
                        :disabled="disabled"
                        :label="item.label"
                        input-align="right"
                        :placeholder="item.helpText"
                    />
                </picker>
            </van-cell-group>
            <van-cell-group custom-class="cell" v-if="item.type=='Year'">
                <picker :disabled="disabled" mode="date" :value="item.value" fields="year"  @change="((e)=>{bindYear(e,item)})">
                    <van-field
                        :value="item.value"
                        input-class="inp"
                        custom-style="font-size:34rpx;color:#333333"
                        :required="item.require||false"
                        :disabled="disabled"
                        :label="item.label"
                        input-align="right"
                        :placeholder="item.helpText"
                    />
                </picker>
            </van-cell-group>
            <van-cell-group custom-class="cell" v-if="item.type=='Month'">
                <picker :disabled="disabled" mode="date" :value="item.value" fields="month"  @change="((e)=>{bindYMonth(e,item)})">
                    <van-field
                        :value="item.value"
                        input-class="inp"
                        custom-style="font-size:34rpx;color:#333333"
                        required
                        :disabled="disabled"
                        :label="item.label"
                        input-align="right"
                        :placeholder="item.helpText"
                    />
                </picker>
            </van-cell-group>
            <van-cell-group custom-class="cell" v-if="item.type=='G'">
                <div class="checkWrap">
                    <van-checkbox-group :value="item.result" @change="((e)=>{changeCheckTag(e,item)})">
                        <div class="checkboxGroup">
                            <van-checkbox :name="v" v-for="(v,i) in item.value" :key="i" custom-class="check" label-class="labels"  shape="square">
                                <p class="tag">{{v}}</p>
                            </van-checkbox>
                        </div>
                    </van-checkbox-group>
                </div>
            </van-cell-group>
            <!-- <van-cell-group custom-class="cell" v-if="item.type=='Text'">
                <div class="box">
                    <p class="label">{{item.label}}:</p>
                    <textarea v-model="item.value" name="" id="" cols="30" rows="10"></textarea>
                </div>
            </van-cell-group> -->
            <!-- <van-checkbox-group :value="result" @change="function(e){onChangeGroup(e)}">
                <van-cell-group custom-class="cell">
                    <van-cell
                        title="复选框1"
                        value-class="value-class"
                        clickable
                        data-index="1"
                        @click="toggle"
                    >
                    <van-checkbox
                        @tap="noop"
                        :class="'checkboxes-'+1"
                        name="1"
                    />
                    </van-cell>
                </van-cell-group>
            </van-checkbox-group> -->
            <van-cell-group custom-class="cell" v-if="item.type=='U'">
                <van-cell value-class="cellValue" title-style="font-size:34rpx;" :title="item.label" is-link :value="item.value" @click="!disabled?getOpenModal(item,index):''" />
            </van-cell-group>
            <div class="switch" v-if="item.type=='H'">
                <p>
                    {{item.label}}
                </p>
                <p>
                    <van-switch :disabled="disabled" :checked="item.value" @change="(val)=>{changeSwitch(val,item)}" size="24px" />
                </p>
            </div>
            <div class="row" v-if="item.type=='UC'">
                <p class="title">*<span>内容</span></p>
                <textarea :disabled="disabled" v-model="item.value" name="" id="" cols="30" rows="10" placeholder-class="placeholder" placeholder="请输入"></textarea>
            </div>
            <div class="parentWrap" v-if="item.type=='RelatedList'">
                <h3>{{item.label}}</h3>
                <div class="rowBox" v-for="(v,i) in item.fields" :key="i">
                    <van-cell-group v-if="v.type=='S'||v.type=='E'||v.type=='N'||v.type=='H'" custom-class="cell">
                        <van-field
                            :value="v.value"
                            custom-style="font-size:34rpx;color:#333333"
                            :required="v.require||false"
                            :label="v.label"
                            :placeholder="v.helpText"
                            input-align="right"
                            @change="function(val){changeInput(val,v)}"
                            :disabled="disabled"
                        />
                    </van-cell-group>
                    <!-- <van-cell-group custom-class="cell" v-if="v.type=='L'||v.type=='DT'||v.type=='LT'">
                        <picker @change="(val)=>{bindPickerChange(val,v)}" :value="v.index" range-key="label" :range="currenData[v.id]">
                            <van-field
                                :value="currenData[v.id][v.index]?currenData[v.id][v.index].label:''"
                                input-class="inp"
                                custom-style="font-size:34rpx;color:#333333"
                                :required="v.require||false"
                                disabled
                                :label="v.label"
                                :placeholder="v.helpText"
                                input-align="right"
                                right-icon="arrow"
                            />
                            </picker>
                    </van-cell-group> -->
                    <van-cell-group custom-class="cell" v-if="v.type=='D'">
                        <picker mode="date" :value="v.value" @change="function(val){bindDateChange(val,v)}">
                            <van-field
                                :value="v.value"
                                title-width="110px"
                                input-class="inp"
                                custom-style="font-size:34rpx;color:#333333"
                                :required="v.require||false"
                                :disabled="disabled"
                                :label="v.label"
                                input-align="right"
                                :placeholder="v.helpText"
                            />
                        </picker>
                    </van-cell-group>
                    <van-cell-group custom-class="cell" v-if="v.type=='O'">
                        <van-cell value-class="cellValue" :title="v.label" is-link :value="v.value" @click="getOpenModal(v,i)" />
                    </van-cell-group>
                    <div class="row" v-if="v.type=='UC'">
                        <p class="title">*<span>{{v.label}}</span></p>
                        <textarea v-model="v.value" name="" id="" cols="30" rows="10" placeholder-class="placeholder" :placeholder="v.helpText"></textarea>
                    </div>
                </div>
            </div>
        </div>
        <van-popup
            :show="isShow"
            position="bottom"
            custom-style="width:100%;height: 80vh;"
            z-index="99999"
            @close="onClosePopup"
        >
            <div class="popupWrap">
                <van-search :value="lksrch" placeholder="请输入搜索关键词" @change="handleChange" />
                <ul class="uls">
                    <li @click="getPopupSel(item,index)" v-for="(item,index) in list[searchIdx]?list[searchIdx].temp:''" :key="index">
                        <p>{{item.Name}}</p>
                        <p v-if="popupIdx==index">
                            <i-icon type="right" />
                        </p>
                    </li>
                </ul>
            </div>
        </van-popup>
    </div>
</template>
<script>
import list from '@/utils/test.js';
import {newMultiArray} from '@/utils/multiArray';
import { mockData,dataList,serachList,testList } from '@/utils/mock';
export default {
    name:"FormList",
    props:['ProcessId','ProcessInstanceId','RuleLogId'],
    data(){
        return {
            title:"通用请示报告",
            index:0,
            level:['普通','紧急','加急'],
            content:"",
            imgList:[],
            ProcessId:"",
            sessionkey:"",
            fullName:"",
            list:[
            ],
            result:[],
            isShow:false,
            popupIdx:-1,
            checked:false,
            currenData:{},
            record:{},
            searchId:"",
            sObjectType:"",
            searchIdx:'',
            ProcessInstanceId:"",
            listData:[],
            lksrch:"",
            agreeShow:false,
            params:{
                processId:"",
                parentRecord:{
                    fields:{}
                },
            },
            testLists:[],
            disabled:true
        }
    },
    computed:{
        isModelmes(){
            return wx.getStorageSync('isModelmes');
        },
        newMultiArrayList(){
            return newMultiArray();
        },
    },
    onLoad(){
        this.testLists = testList.listData;
        let sessionkey = wx.getStorageSync('sessionkey');
        this.sessionkey = sessionkey;
        this.params.processId = this.ProcessId;
        this.getQueryFrom();
        this.$nextTick(()=>{
            this.getLayoutData();
        })
    },
    methods:{
        getQueryFrom(){
            this.$httpWX.get({
                url:this.$api.message.queryList,
                data:{
                    method:this.$api.approval.mobileform,
                    SessionKey:this.sessionkey,
                    ProcessId:this.ProcessId
                }
            }).then(res=>{
                console.log(res);
                this.list = res.layout;
                this.list.forEach(item=>{
                    this.$set(item,'value','');
                    this.$set(item,'require',false);
                    if(item.type=='L'){
                        this.$set(item,'index','');
                    }
                })
            })
        },
        getLayoutData(){
            this.$httpWX.get({
                url:this.$api.message.queryList,
                data:{
                    method:this.$api.approval.layoutData,
                    SessionKey:this.sessionkey,
                    processId:this.ProcessId,
                    ProcessInstanceId:this.ProcessInstanceId,
                    RuleLogId:this.RuleLogId
                }
            }).then(res=>{
                console.log(res);
                this.currenData = res.actions[0].returnValue.masterRecord.picklistValuesMap;
                this.record = res.actions[0].returnValue.masterRecord.record;
                this.list.forEach(item=>{
                    if( this.record[item.name] instanceof Object){
                        item.value = this.record[item.name].Name;
                    }
                    else if(item.type=='L'||item.type=='DT'||item.type=='LT'){
                       item.index = this.currenData[item.id].findIndex(v=>v.value==this.record[item.name])
                    }
                    else {
                        item.value = this.record[item.name];
                    }
                    
                })
            })
        },
        getOpenModal(item,index){
            this.lksrch = '';
            this.popupIdx = -1;
            this.searchId = item.id;
            this.searchIdx = index;
            this.sObjectType = item.sObjectType;
            this.getLookup().then(res=>{
                item.temp = res.listData;
                this.isShow = true;
            });
        },
        handleChange(e){
            this.lksrch = e.mp.detail;
            this.getLookup().then(res=>{
                this.list[this.searchIdx].temp = res.listData;
            });
        },
        getLookup(){
            return new Promise((resolve,reject)=>{
                this.$httpWX.get({
                    url:this.$api.message.queryList,
                    data:{
                        method:this.$api.approval.lookup,
                        SessionKey:this.sessionkey,
                        lktp:this.sObjectType,
                        lksrch:this.lksrch
                    }
                }).then(res=>{
                    this.listData = res.listData;
                    resolve(res);
                })
            })
        },
        getPopupSel(item,index){
            this.popupIdx = index;
            this.isShow = false;
            this.list[this.searchIdx].value = item.Name;
            this.params.parentRecord.fields[this.searchId] = item.ID;
        },
        onClosePopup(){
            this.isShow =  false;
        },
         changeInput(val,item){
            console.log(val,item);
            item.value =  val.mp.detail;
            this.record[item.id] = item.value;
            this.params.parentRecord.fields[item.id]= item.value;
            // console.log(this.record)
        },
        onChangeGroup(e){
            console.log(e,'123123123');
            this.result = e.mp.detail;
             this.params.parentRecord.fields[item.id] = this.result;
        },
        toggle(e){
            console.log(e);
            let { index } = e.mp.currentTarget.dataset;
        },
        noop() {},
                changeSwitch(e,item){
            console.log(e,item);
            item.value = e.mp.detail;
            this.params.parentRecord.fields[item.id] = e.mp.detail;
        },
        bindPickerChange(val,item){
            item.index = val.mp.detail.value;
            console.log(this.currenData[item.id][item.index].label);
            item.value = this.currenData[item.id][item.index].value;
            this.record[item.id] = this.currenData[item.id][item.index].value;
            this.params.parentRecord.fields[item.id] = item.value;
        },
        bindDateChange(v,item){
            console.log(v,item);
            item.value = v.mp.detail.value;
            this.record[item.id] = item.value;
            this.params.parentRecord.fields[item.id] = item.value;
        },
        bindMultiPickerChange(v,item){
            item.multiIndex = v.mp.detail.value;
            item.value = this.getCurrentTime(item.multiIndex);
            this.params.parentRecord.fields[item.id] = item.value;
        },
        bindYear(e,item){
            console.log(e,item);
            item.value = e.mp.detail.value;
            this.record[item.id] = item.value;
            this.params.parentRecord.fields[item.id] = item.value;
        },
        bindYMonth(e,item){
            item.value = e.mp.detail.value;
            this.record[item.id] = item.value;
            this.params.parentRecord.fields[item.id] = item.value;
        },
        getCurrentTime(index){
            const year = this.newMultiArrayList[ 0 ][ index[ 0 ] ];
            const month = this.newMultiArrayList[ 1 ][ index[ 1 ] ];
            const day = this.newMultiArrayList[ 2 ][ index[ 2 ] ];
            const hour = this.newMultiArrayList[ 3 ][ index[ 3 ] ];
            const minute = this.newMultiArrayList[ 4 ][ index[ 4 ] ];
            let time = year + "-" + month + "-" + day + " " + hour + ":" + minute;
            time = this.RemoveChinese(time);
            return time;
        },
        RemoveChinese(strValue) {  
            if(strValue!= null && strValue != ""){  
                var reg = /[\u4e00-\u9fa5]/g;   
               return strValue.replace(reg, "");   
            }  
            else  
                return "";  
        },
        changeCheckTag(e,item){
            console.log(e,item);
            item.result = e.mp.detail;
        }
    }
}
</script>
<style lang="scss">
.van-cell__title{
    font-size: 34rpx!important;
}
    .wrap_w{
        width: 100%;
        height: 100%;
        overflow: hidden;
        .parentWrap{
            width: 100%;
            height: auto;
            background: #fff;
            margin: 30rpx 0;
            padding: 30rpx 0;
            h3{
                font-weight: bold;
                text-align: center;
            }
        }
        .value-class {
            flex: none !important;
        }
        .cell{
            margin-top: 24rpx;
            .cellValue{
                font-size: 34rpx;
                color: #333333;
            }
            .inp{
                color: #333333;
                font-size: 34rpx;
            }
            .box{
                width: 100%;
                background: #fff;
                padding: 30rpx;
                textarea{
                    width: 100%;
                    // color: #999999;
                }
            }
            .checkWrap{
                background: #fff;
                padding: 30rpx;
                .check{
                    padding: 10rpx 0;
                    .tag{
                        width: 500rpx;
                    }
                }
            }
        }
        .switch{
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 33rpx;
            background: #fff;
            margin-top: 24rpx;
        }
        .row{
            margin-top: 24rpx;
            background: #fff;
            padding:15rpx;
            .title{
                color: #ff6666;
                span{
                    font-size: 34rpx;
                    color: #333333;
                    margin-left: 5rpx;
                }
            }
            .text{
                font-size: 34rpx;
                color: #333333;
                margin-left: 10rpx;
            }
            textarea{
                width: 90%;
                height: 150rpx;
                padding: 10rpx;
            }
            .placeholder{
                font-size: 34rpx;
                color: #a3a5a7;
            }
            .content{
                display: flex;
                padding: 20rpx 10rpx;
                .box{
                    width: 96rpx;
                    height: 96rpx;
                    display: flex;
                    justify-content: center;
                    align-items: center;
                    background: #f7f7f7;
                    margin-right: 10rpx;
                    img{
                        width: 100%;
                        height: 100%;
                        vertical-align: middle;
                    }
                }
            }
        }
        .footer{
            width: 100%;
            position: fixed;
            bottom: 0;
            background: #fff;
            z-index: 999;
            .btn{
                padding: 24rpx;
            }
        }
        .popupWrap{
            .uls{
                li{
                    padding: 33rpx ;
                    color: #333333;
                    font-size: 28rpx;
                    display: flex;
                    justify-content: space-between;
                }
            }
        }
        .agreeWrap{
            position: relative;
            background: #f2f2f2;
            .header{
                display: flex;
                padding: 30rpx;
                border-bottom: 1rpx solid #e2e4e3;
                width: 100%;
                background: #fff;
                // position: fixed;
                .radius{
                    width: 83rpx;
                    height: 83rpx;
                    line-height: 83rpx;
                    text-align: center;
                    border-radius: 50%;
                    font-size: 26rpx;
                    color: #fff;
                    background: #3399ff;
                }
                div:nth-child(2){
                    margin-left: 20rpx;
                    h3{
                        font-size: 28rpx;
                        font-weight:bold;
                    }
                    p{
                        font-size: 12px;
                        color: #333333;
                        margin-top: 10rpx;
                        span{
                            color: #666666;
                        }
                    }
                }
            }
            .cont{
                width: 100%;
                // position: absolute;
                // top: 70px;
                height: 400px;
                overflow: scroll;
                padding-bottom: 50px;
                h3{
                    background: #f2f2f2;
                    font-size: 12px;
                    color: #999999;
                    padding: 20rpx;
                }
                .box{
                    padding: 20rpx;
                    background: #fff;
                    .row{
                        padding: 20rpx 0;
                        font-size: 28rpx;
                        border-bottom: 1rpx solid #e2e4e3;
                        color: #333333;
                        display: flex;
                        justify-content: space-between;
                    }
                    .rows{
                        padding: 10rpx 0 0 0;
                        font-size: 28rpx;
                        color: #333333;
                        display: flex;
                        justify-content: center;
                        .add{
                            // text-align: center;
                            color: #3399ff;
                            span{
                                margin-top: 10rpx;
                                display: inline-block;
                            }
                        }
                    }
                }
                .popupRow{
                    .row{
                        padding: 20rpx;
                        color: #999999;
                        font-size: 12px;
                        background: #f2f2f2;
                        display: flex;
                        justify-content: space-between;
                        p:nth-child(2){
                            span:nth-child(1){
                                display: inline-block;
                                background: #3399ff;
                                border-top-left-radius: 10rpx;
                                border-bottom-left-radius: 10rpx;
                                padding: 10rpx 20rpx;
                                color: #fff;
                            }
                            span:nth-child(2){
                                display: inline-block;
                                background: #fff;
                                border-top-right-radius: 10rpx;
                                border-bottom-right-radius: 10rpx;
                                padding: 10rpx 20rpx;
                                color: #3399ff;
                                border: 1rpx solid #3399ff;
                                box-sizing: border-box;
                                border-left: none;
                            }
                        }
                    }
                    .text{
                        padding: 20rpx;
                        background: #fff;
                        font-size: 12px;
                        color: #333333;
                    }
                }
                .textarea{
                    padding: 20rpx;
                    background: #fff;
                    margin-top: 20rpx ;
                    p{
                        font-size: 28rpx;
                        font-weight: bold;
                    }
                    textarea{
                        height: 80px;
                    }
                }
            }
            .fot{
                width: 100%;
                position: fixed;
                bottom: 0;
                background: #fff;
                .box{
                    display:flex;
                    justify-content: center;
                    p{
                        width: 33.3%;
                        text-align: center;
                        color: #3399ff;
                        padding: 20rpx;
                        border-right: 1rpx solid #f2f2f2;
                    }
                }

            }
        }
    }
</style>