<template>
  <div class="dashboard-editor-container">
    <github-corner></github-corner>
    <panel-group :data="data"></panel-group>
<iframe width="600" scrolling="no" height="100" frameborder="0" allowtransparency="true" src="//i.tianqi.com/index.php?c=code&id=12&icon=1&py=jinyun&num=3&site=12"></iframe>
    <d-chart></d-chart>
  </div>
</template>
<script>
  import GithubCorner from '@/components/GithubCorner'
  import PanelGroup from './components/PanelGroup'
  import DChart from './components/DChart'
  import {
    getShopCountDetail
  } from '@/api/shouye'
import { getToken } from '@/utils/auth'
  export default {
    name: 'dashboard-admin',
    components: {
      GithubCorner,
      PanelGroup,
      DChart
    },
    data() {
      return {
        data: {
          saleoutproducts: '',
          deliverySO: '',
          questionSO: '',
          shenheCash: ''
        },
        request: {
          authorization: ''
        }
      }
    },
    created() {
      this.getList()
    },
    methods: {
      getList() {
        this.request.authorization = getToken('Admin-Token')
        getShopCountDetail(this.request).then(response => {
          this.data = response.data.data
          console.log(this.data)
          console.log(response)
        })
      }
    }
  }
</script>

<style rel="stylesheet/scss" lang="scss" scoped>
  .dashboard-editor-container {
    padding: 32px;
    background-color: rgb(240, 242, 245);
    .chart-wrapper {
      background: #fff;
      padding: 16px 16px 0;
      margin-bottom: 32px;
    }
  }
</style>
