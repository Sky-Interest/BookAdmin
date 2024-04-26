
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

  <a-space size="small">
    <router-link to="/addBook">
      <a-button style="margin-left: 50px">
        添加图书信息
      </a-button>
    </router-link>
  </a-space>


  <a-table :columns="columns"
           :data-source="data"
           :pagination="pagination"
           @change="handleTableChange"
  >
    <template #indexDraw="{ index }">
      {{ covering(index+1) }}
    </template>
    <template #bodyCell="{ column, text,record }" >
      <template v-if="column.dataIndex === 'categoryName'" >
        <a @click="updateBook(record)">{{ text }}</a>
      </template>
    </template>
  </a-table>
</template>

<script lang="ts">
import { defineComponent,onMounted ,ref} from 'vue';
import axios from "axios";
import {SelectProps} from "ant-design-vue";
import moment from "moment";
import {useRouter} from "vue-router";

const columns = [
  {
    title: '序号',
    dataIndex: 'indexDraw',
    fixed: 'left',
    slots: { customRender: 'indexDraw' },
  },
  {
    title: '图书名称',
    dataIndex: 'name'
  },
  {
    title: '出版日期',
    dataIndex: 'publishTime',
    customRender:(text:any,row:any,index:any)=>{
      return moment(text).format('YYYY-MM-DD')
    }

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
    dataIndex: 'categoryName',
  }
];

const data = ref();

export default defineComponent({
  name: 'HomeView',
  setup() {
    const router = useRouter();
    const pagination = ref({
      current: 1,
      pageSize: 5,
      total: 0
    });

    const covering = (index:number)=>{
      return String(index).padStart(2, '0');
    };

    const updateBook = (book:any)=>{
      router.push({
        path: '/updateBook',
        query: { data:JSON.stringify(book) }
      })
    }


    const queryBookList = (params:any)=>{
      axios.get("http://localhost:8080/getBookList",
          {
            params:{
              page:params.page,
              size:params.size,
              id:categoryId.value
            }}).then(function (resp){
        console.log(resp.data);
        data.value = resp.data.list;
        //重置分页按钮
        pagination.value.current = params.page;
        pagination.value.total = resp.data.total;
      })
    }

    const category = ref<SelectProps['options']>([]);
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

    /*
    * 表格点击页码时触发
    * */
    const handleTableChange =(pagination:any)=>{
      queryBookList({
        page:pagination.current,
        size:pagination.pageSize
      });
    };

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

    })
    return {
      data,
      columns,
      category,
      categoryId,
      query,
      pagination,
      handleTableChange,
      updateBook,
      covering
    };
  },
});
</script>
