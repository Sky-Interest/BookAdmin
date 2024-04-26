
<template>
  按图书类别查询：
  <a-select
      v-model:value="categoryId"
      style="width: 120px"
      :options="category"
      placeholder="不限类型"
  ></a-select>
  <a-button type="primary"  @click="query()" style="margin-left: 10px">
    查询
  </a-button >

  <a-table :columns="columns" :data-source="data" :pagination="pagination" @change="handleTableChange">
    <template #name="{ text }">
      <a>{{ text }}</a>
    </template>
  </a-table>
<!--  {{book}}-->
</template>
<script lang="ts">
import { defineComponent,onMounted,ref } from 'vue';
import axios from "axios";
import { selectProps } from 'ant-design-vue/es/vc-select';

const columns = [
  {
    title: '序号',
    dataIndex: 'id'
  },
  {
    title: '图书名称',
    dataIndex: 'name'
  },
  {
    title: '出版日期',
    dataIndex: 'publishTime'
  },
  {
    title: '出版社',
    dataIndex: 'publishName'
  },
  {
    title: '价格',
    dataIndex: 'price'
  },
  {
    title: '图书类别',
    dataIndex: 'categoryId'
  },
];

const data = ref();

export default defineComponent({
  setup() {

    const pagination = ref({
      current:1,
      pageSize:5,
      total:0
    });

    const query = ()=>{
      queryBookList({
        page:1,
        size:pagination.value.pageSize
      });
    }

    onMounted(()=>{
      queryBookList({
        page:1,
        size:pagination.value.pageSize
      });
      queryBookCategoryList();
    });
    //表格点击页码时触发
    const handleTableChange = (pagination:any)=>{
      queryBookList({
        page:pagination.current,
        size:pagination.pageSize
      });
    };



    const queryBookList = (params:any)=>{
      axios.get("http://localhost:8080/getBookList",{
        params:{
          page:params.page,
          size:params.size,
          id:categoryId.value,
        }
      }).then(function (resp){
        // console.log(resp);
        data.value=resp.data;
        //重置分页
        pagination.value.current = params.page;
        pagination.value.total = resp.data.total;

      })
    }


    const category = ref<typeof selectProps[]>([]);
    const categoryId = ref();

    const queryBookCategoryList = ()=>{
      axios.get("http://localhost:8080/getBookListCategory").then(function (resp){
        const data = resp.data;
        data.unshift({id: '' , name: '不限类型'});
        console.log(data)
        category.value = data.map((item: {id: number, name: string}) => ({
          value: item.id ,
          label: item.name
        }));
      })
    }

    return {
      data,
      columns,
      query,
      category,
      categoryId,
      pagination,
      handleTableChange,

    };
  },
});
</script>