<!--<template>-->
<!--  <a-card :bordered="false">-->
<!--    &lt;!&ndash; 查询区域 &ndash;&gt;-->
<!--    <div class="table-page-search-wrapper">-->
<!--      <a-form layout="inline" @keyup.enter.native="searchQuery">-->
<!--        <a-row :gutter="24">-->
<!--          <a-col :xl="6" :lg="7" :md="8" :sm="24">-->
<!--            <a-form-item label="用户账号">-->
<!--              <a-input placeholder="请输入用户账号" v-model="queryParam.userAcc"></a-input>-->
<!--            </a-form-item>-->
<!--          </a-col>-->
<!--          <a-col :xl="6" :lg="7" :md="8" :sm="24">-->
<!--            <a-form-item label="用户姓名">-->
<!--              <a-input placeholder="请输入用户姓名" v-model="queryParam.userName"></a-input>-->
<!--            </a-form-item>-->
<!--          </a-col>-->
<!--          <template v-if="toggleSearchStatus">-->
<!--            <a-col :xl="6" :lg="7" :md="8" :sm="24">-->
<!--              <a-form-item label="所属部门">-->
<!--                <a-input placeholder="请输入所属部门" v-model="queryParam.userDep"></a-input>-->
<!--              </a-form-item>-->
<!--            </a-col>-->
<!--            <a-col :xl="6" :lg="7" :md="8" :sm="24">-->
<!--              <a-form-item label="性别">-->
<!--                <a-input placeholder="请输入性别" v-model="queryParam.userSex"></a-input>-->
<!--              </a-form-item>-->
<!--            </a-col>-->
<!--          </template>-->
<!--          <a-col :xl="6" :lg="7" :md="8" :sm="24">-->
<!--            <span style="float: left;overflow: hidden;" class="table-page-search-submitButtons">-->
<!--              <a-button type="primary" @click="searchQuery" icon="search">查询</a-button>-->
<!--              <a-button type="primary" @click="searchReset" icon="reload" style="margin-left: 8px">重置</a-button>-->
<!--              <a @click="handleToggleSearch" style="margin-left: 8px">-->
<!--                {{ toggleSearchStatus ? '收起' : '展开' }}-->
<!--                <a-icon :type="toggleSearchStatus ? 'up' : 'down'"/>-->
<!--              </a>-->
<!--            </span>-->
<!--          </a-col>-->
<!--        </a-row>-->
<!--      </a-form>-->
<!--    </div>-->
<!--    &lt;!&ndash; 查询区域-END &ndash;&gt;-->
<!--    -->
<!--    &lt;!&ndash; 操作按钮区域 &ndash;&gt;-->
<!--    <div class="table-operator">-->
<!--      <a-button @click="handleAdd" type="primary" icon="plus">新增</a-button>-->
<!--      <a-button type="primary" icon="download" @click="handleExportXls('用户信息')">导出</a-button>-->
<!--      <a-upload name="file" :showUploadList="false" :multiple="false" :headers="tokenHeader" :action="importExcelUrl" @change="handleImportExcel">-->
<!--        <a-button type="primary" icon="import">导入</a-button>-->
<!--      </a-upload>-->
<!--      <a-dropdown v-if="selectedRowKeys.length > 0">-->
<!--        <a-menu slot="overlay">-->
<!--          <a-menu-item key="1" @click="batchDel"><a-icon type="delete"/>删除</a-menu-item>-->
<!--        </a-menu>-->
<!--        <a-button style="margin-left: 8px"> 批量操作 <a-icon type="down" /></a-button>-->
<!--      </a-dropdown>-->
<!--    </div>-->

<!--    &lt;!&ndash; table区域-begin &ndash;&gt;-->
<!--    <div>-->
<!--      <div class="ant-alert ant-alert-info" style="margin-bottom: 16px;">-->
<!--        <i class="anticon anticon-info-circle ant-alert-icon"></i> 已选择 <a style="font-weight: 600">{{ selectedRowKeys.length }}</a>项-->
<!--        <a style="margin-left: 24px" @click="onClearSelected">清空</a>-->
<!--      </div>-->

<!--      <a-table-->
<!--        ref="table"-->
<!--        size="middle"-->
<!--        bordered-->
<!--        rowKey="id"-->
<!--        :columns="columns"-->
<!--        :dataSource="dataSource"-->
<!--        :pagination="ipagination"-->
<!--        :loading="loading"-->
<!--        :rowSelection="{selectedRowKeys: selectedRowKeys, onChange: onSelectChange}"-->
<!--        class="j-table-force-nowrap"-->
<!--        @change="handleTableChange">-->

<!--        <template slot="htmlSlot" slot-scope="text">-->
<!--          <div v-html="text"></div>-->
<!--        </template>-->
<!--        <template slot="imgSlot" slot-scope="text">-->
<!--          <span v-if="!text" style="font-size: 12px;font-style: italic;">无图片</span>-->
<!--          <img v-else :src="getImgView(text)" height="25px" alt="" style="max-width:80px;font-size: 12px;font-style: italic;"/>-->
<!--        </template>-->
<!--        <template slot="fileSlot" slot-scope="text">-->
<!--          <span v-if="!text" style="font-size: 12px;font-style: italic;">无文件</span>-->
<!--          <a-button-->
<!--            v-else-->
<!--            :ghost="true"-->
<!--            type="primary"-->
<!--            icon="download"-->
<!--            size="small"-->
<!--            @click="uploadFile(text)">-->
<!--            下载-->
<!--          </a-button>-->
<!--        </template>-->

<!--        <span slot="action" slot-scope="text, record">-->
<!--          <a @click="handleEdit(record)">编辑</a>-->

<!--          <a-divider type="vertical" />-->
<!--          <a-dropdown>-->
<!--            <a class="ant-dropdown-link">更多 <a-icon type="down" /></a>-->
<!--            <a-menu slot="overlay">-->
<!--              <a-menu-item>-->
<!--                <a-popconfirm title="确定删除吗?" @confirm="() => handleDelete(record.id)">-->
<!--                  <a>删除</a>-->
<!--                </a-popconfirm>-->
<!--              </a-menu-item>-->
<!--            </a-menu>-->
<!--          </a-dropdown>-->
<!--        </span>-->

<!--      </a-table>-->
<!--    </div>-->

<!--    <userRb-modal ref="modalForm" @ok="modalFormOk"></userRb-modal>-->
<!--  </a-card>-->
<!--</template>-->

<!--<script>-->

<!--  import '@/assets/less/TableExpand.less'-->
<!--  import { mixinDevice } from '@/utils/mixin'-->
<!--  import { JeecgListMixin } from '@/mixins/JeecgListMixin'-->
<!--  import UserRbModal from './modules/UserRbModal'-->

<!--  export default {-->
<!--    name: "UserRbList",-->
<!--    mixins:[JeecgListMixin, mixinDevice],-->
<!--    components: {-->
<!--      UserRbModal-->
<!--    },-->
<!--    data () {-->
<!--      return {-->
<!--        description: '用户信息管理页面',-->
<!--        // 表头-->
<!--        columns: [-->
<!--          {-->
<!--            title: '#',-->
<!--            dataIndex: '',-->
<!--            key:'rowIndex',-->
<!--            width:60,-->
<!--            align:"center",-->
<!--            customRender:function (t,r,index) {-->
<!--              return parseInt(index)+1;-->
<!--            }-->
<!--          },-->
<!--          {-->
<!--            title:'用户账号',-->
<!--            align:"center",-->
<!--            dataIndex: 'userAcc'-->
<!--          },-->
<!--          {-->
<!--            title:'用户姓名',-->
<!--            align:"center",-->
<!--            dataIndex: 'userName'-->
<!--          },-->
<!--          {-->
<!--            title:'所属部门',-->
<!--            align:"center",-->
<!--            dataIndex: 'userDep'-->
<!--          },-->
<!--          {-->
<!--            title: '操作',-->
<!--            dataIndex: 'action',-->
<!--            align:"center",-->
<!--            // fixed:"right",-->
<!--            width:147,-->
<!--            scopedSlots: { customRender: 'action' }-->
<!--          }-->
<!--        ],-->
<!--        url: {-->
<!--          list: "/userrb/userRb/list",-->
<!--          delete: "/userrb/userRb/delete",-->
<!--          deleteBatch: "/userrb/userRb/deleteBatch",-->
<!--          exportXlsUrl: "/userrb/userRb/exportXls",-->
<!--          importExcelUrl: "userrb/userRb/importExcel",-->
<!--        },-->
<!--        dictOptions:{},-->
<!--      }-->
<!--    },-->
<!--    computed: {-->
<!--      importExcelUrl: function(){-->
<!--        return `${window._CONFIG['domianURL']}/${this.url.importExcelUrl}`;-->
<!--      },-->
<!--    },-->
<!--    methods: {-->
<!--      initDictConfig(){-->
<!--      }-->
<!--    }-->
<!--  }-->
<!--</script>-->
<!--<style scoped>-->
<!--  @import '~@assets/less/common.less';-->
<!--</style>-->

<template>
  <a-card :bordered="false">
    <!-- 查询区域 -->
    <div class="table-page-search-wrapper">
      <a-form layout="inline" @keyup.enter.native="searchQuery">
        <a-row :gutter="24">
          <a-col :xl="6" :lg="7" :md="8" :sm="24">
            <a-form-item label="用户账号">
              <a-input placeholder="请输入用户账号" v-model="queryParam.userAcc"></a-input>
            </a-form-item>
          </a-col>
          <a-col :xl="6" :lg="7" :md="8" :sm="24">
            <a-form-item label="用户姓名">
              <a-input placeholder="请输入用户姓名" v-model="queryParam.userName"></a-input>
            </a-form-item>
          </a-col>
          <template v-if="toggleSearchStatus">
            <a-col :xl="6" :lg="7" :md="8" :sm="24">
              <a-form-item label="所属部门">
                <j-multi-select-tag placeholder="请选择所属部门" dictCode="user_dep" v-model="queryParam.userDep"/>
              </a-form-item>
            </a-col>
            <a-col :xl="6" :lg="7" :md="8" :sm="24">
              <a-form-item label="性别">
                <j-dict-select-tag placeholder="请选择性别" v-model="queryParam.userSex" dictCode="user_sex"/>
              </a-form-item>
            </a-col>
          </template>
          <a-col :xl="6" :lg="7" :md="8" :sm="24">
            <span style="float: left;overflow: hidden;" class="table-page-search-submitButtons">
              <a-button type="primary" @click="searchQuery" icon="search">查询</a-button>
              <a-button type="primary" @click="searchReset" icon="reload" style="margin-left: 8px">重置</a-button>
              <a @click="handleToggleSearch" style="margin-left: 8px">
                {{ toggleSearchStatus ? '收起' : '展开' }}
                <a-icon :type="toggleSearchStatus ? 'up' : 'down'"/>
              </a>
            </span>
          </a-col>
        </a-row>
      </a-form>
    </div>
    <!-- 查询区域-END -->

    <!-- 操作按钮区域 -->
    <div class="table-operator">
      <a-button @click="handleAdd" type="primary" icon="plus">新增</a-button>
      <a-button type="primary"  @click="">回收站</a-button>
      <a-button type="primary"  @click="">批量操作</a-button>
      <!--      handleExportXls('用户信息')-->
      <!--      <a-upload name="file" :showUploadList="false" :multiple="false" :headers="tokenHeader" :action="importExcelUrl" @change="handleImportExcel">-->
      <!--        <a-button type="primary" icon="import">导入</a-button>-->
      <!--      </a-upload>-->
      <a-dropdown v-if="selectedRowKeys.length > 0">
        <a-menu slot="overlay">
          <a-menu-item key="1" @click="batchDel"><a-icon type="delete"/>删除</a-menu-item>
          <a-menu-item key="2" @click="batchDel"><a-icon type="delete"/>编辑</a-menu-item>
          <a-menu-item key="3" @click="batchDel"><a-icon type="delete"/>密码</a-menu-item>
          <a-menu-item key="4" @click="batchDel"><a-icon type="delete"/>冻结</a-menu-item>
          <a-menu-item key="5" @click="batchDel"><a-icon type="delete"/>打印</a-menu-item>
        </a-menu>
        <!--        <a-button style="margin-left: 8px"> 批量操作 <a-icon type="down" /></a-button>-->
      </a-dropdown>
    </div>

    <!-- table区域-begin -->
    <div>
      <div class="ant-alert ant-alert-info" style="margin-bottom: 16px;">
        <i class="anticon anticon-info-circle ant-alert-icon"></i> 已选择 <a style="font-weight: 600">{{ selectedRowKeys.length }}</a>项
        <a style="margin-left: 24px" @click="onClearSelected">清空</a>
      </div>

      <a-table
        ref="table"
        size="middle"
        bordered
        rowKey="id"
        :columns="columns"
        :dataSource="dataSource"
        :pagination="ipagination"
        :loading="loading"
        :rowSelection="{selectedRowKeys: selectedRowKeys, onChange: onSelectChange}"
        class="j-table-force-nowrap"
        @change="handleTableChange">

        <template slot="htmlSlot" slot-scope="text">
          <div v-html="text"></div>
        </template>
        <template slot="imgSlot" slot-scope="text">
          <span v-if="!text" style="font-size: 12px;font-style: italic;">无图片</span>
          <img v-else :src="getImgView(text)" height="25px" alt="" style="max-width:80px;font-size: 12px;font-style: italic;"/>
        </template>
        <template slot="fileSlot" slot-scope="text">
          <span v-if="!text" style="font-size: 12px;font-style: italic;">无文件</span>
          <a-button
            v-else
            :ghost="true"
            type="primary"
            icon="download"
            size="small"
            @click="uploadFile(text)">
            下载
          </a-button>
        </template>

        <span slot="action" slot-scope="text, record">
          <a @click="handleEdit(record)">编辑</a>

          <a-divider type="vertical" />
          <a-dropdown>
            <a class="ant-dropdown-link">更多 <a-icon type="down" /></a>
            <a-menu slot="overlay">
              <a-menu-item>
                <a-popconfirm title="确定删除吗?" @confirm="() => handleDelete(record.id)">
                  <a>删除</a>
                </a-popconfirm>
              </a-menu-item>
            </a-menu>
          </a-dropdown>
        </span>

      </a-table>
    </div>

    <userInf-modal ref="modalForm" @ok="modalFormOk"></userInf-modal>
  </a-card>
</template>

<script>

  export default {
    components: {},
    data() {
      return {
        form: this.$form.createForm(this),
        selectValue:"",
      }
    },
    methods:{
      getFormFieldValue(field){
        return this.form.getFieldValue(field)
      },
      popupCallback(row){
        this.form.setFieldsValue(row)
      }
    }
  }
</script>