<template>
  <div>
    <a-card :loading="loading" :bordered="false" :body-style="{padding: '0 10px',minWidth:'1200px'}">
      <a-tabs
        default-active-key="1"
        size="large"
        :tab-bar-style="{marginBottom: '12px', paddingLeft: '0'}"
      >
        <a-tab-pane loading="true" :tab="title" key="1" style="overflow:auto">
          <ContractInfo :dataSource="iniDataSource" v-if="moduleType < 4"/>
          <div class="contractType" v-else>
            <a-tabs type="card">
              <a-tab-pane tab="合同信息" key="1">
                <ContractInfo :dataSource="iniDataSource"/>
                <!-- <p>Content of Tab Pane 1</p>
                <p>Content of Tab Pane 1</p>
                <p>Content of Tab Pane 1</p> -->
              </a-tab-pane>
              <a-tab-pane tab="子合同" key="2">
                
                <p>Content of Tab Pane 2</p>
                <p>Content of Tab Pane 2</p>
                <p>Content of Tab Pane 2</p>
              </a-tab-pane>
              <a-tab-pane tab="合同款项" key="3">
                <ContractFund />
              </a-tab-pane>
            </a-tabs>
          </div>
        </a-tab-pane>
        <a slot="tabBarExtraContent" @click="backList">
          <a-icon type="rollback"/>返回合同列表
        </a>
      </a-tabs>
    </a-card>
  </div>
</template>

<script>
import { getServiceList } from '@/api/manage'

import employeeTree from '@/components/same/employeeTree'
import ContractInfo from '@/components/contract/ContractInfo'
import ContractFund from '@/components/contract/ContractFund'

const provinceData = ['Zhejiang', 'Jiangsu']
const cityData = {
  Zhejiang: ['Hangzhou', 'Ningbo', 'Wenzhou'],
  Jiangsu: ['Nanjing', 'Suzhou', 'Zhenjiang']
}

// const booleanFalse = false

export default {
  name: 'TableDetail',
  props: {
    title: {
      type: String,
      default: ''
    },
    moduleType: {
      type: Number,
      default: 2
    }
  },
  components: {
    employeeTree,
    ContractInfo,
    ContractFund
  },
  data() {
    return {
      loading: false,
      type: this.moduleType,
      provinceData,
      cityData,
      areas: cityData[provinceData[0]],
      cities: cityData[provinceData[0]],
      secondCity: cityData[provinceData[0]][0],
      booleanFalse: true,
      iniDataSource: { d: 3, df: 3 },
      visible: false,
      data: parameter => {
        return getServiceList(Object.assign(parameter, this.queryParam)).then(res => {
          console.log(res, 'res')
          return res.result
        })
      }
    }
  },
  created() {
    //数据请求参数配置
  },
  methods: {
    //接口获取数据
    loadData() {},
    backList() {
      console.log(1)
      this.$emit('changeCom', { type: 1 })
    }
  },
  watch: {}
}
</script>

<style>
.ant-form-item-required:before {
  display: inline-block;
  margin-right: 4px;
  content: '*';
  font-family: SimSun;
  line-height: 1;
  font-size: 14px;
  color: #f5222d;
}
.ellipsis {
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}
.ant-tabs-extra-content {
  line-height: 55px;
}

.divItem {
  padding: 10px 0;
}
table td {
  padding: 4px 0px;
}
table th {
  padding: 4px 0px;
  text-align: right;
}
.borderBottom {
  border-bottom: 1px solid #e8e8e8;
}
.sameWidth {
  width: 260px;
}
.th-right {
  text-align: right;
}
.th-top {
  /* text-align: top !important; */
  padding-top: 9px !important;
  vertical-align: top;
}
.label-title {
  margin: 30px 0 10px 0;
  color: #555;
}

.contractType .ant-tabs-card .ant-tabs-content .ant-tabs-tabpane {
  background: #fff;
  /* padding: 16px; */
}

.contractType .ant-tabs-nav-container .ant-tabs-nav-wrap{
  background: #eee;
}
.contractType .ant-tabs-card .ant-tabs-bar {
  border-color: #fff;
}

.contractType .ant-tabs-card .ant-tabs-bar .ant-tabs-tab {
  border-color: #ddd;
  background: #eee;
  margin-right: 0;
  border-radius: 0;
  border:0;
  border-top: 1px solid #ddd;
  border-right: 1px solid #ddd;
}
.contractType .ant-tabs-card .ant-tabs-bar .ant-tabs-tab:nth-child(1){
  border-left: 1px solid #ddd;
}

.contractType .ant-tabs-card .ant-tabs-bar .ant-tabs-tab-active {
  /* border-color: #fff; */
  background: #fff;
}
</style>
