<!--<template>-->
<!--  <a-form-->
<!--      :model="book"-->
<!--      name="basic"-->
<!--      :label-col="{ span: 8 }"-->
<!--      :wrapper-col="{ span: 8 }"-->
<!--      autocomplete="off"-->
<!--  >-->
<!--    <a-form-item-->
<!--        label="图书类型"-->
<!--        name="categoryId"-->
<!--        :rules="[{ required: true, message: '请选择图书类型' }]"-->
<!--    >-->
<!--      <a-select-->
<!--          v-model:value="book.categoryId"-->
<!--          style="width: 510px;text-align: left;"-->
<!--          :options="category"-->
<!--      ></a-select>-->
<!--    </a-form-item>-->

<!--    <a-form-item-->
<!--        label="图书名称"-->
<!--        name="name"-->
<!--        :rules="[{ required: true, message: '请输入图书名称' }]"-->
<!--    >-->
<!--      <a-input v-model:value="book.name" />-->
<!--    </a-form-item>-->

<!--    <a-form-item-->
<!--      label="上架时间"-->
<!--      name="selfTime">-->
<!--      <a-input v-model:value="book.selfTime" disabled/>-->
<!--    </a-form-item>-->

<!--    <a-form-item-->
<!--        label="出版日期"-->
<!--        name="publishTime"-->
<!--        :rules="[{ required: true, message: '请输入出版日期' }]"-->
<!--    >-->
<!--      <a-date-picker v-model:value="book.publishTime" style="width: 510px"/>-->
<!--    </a-form-item>-->

<!--    <a-form-item-->
<!--        label="价格"-->
<!--        name="price"-->
<!--        :rules="[{ required: true,-->
<!--        // pattern: /([1-9]([0-9]+)?(.[0-9]{1,2})?$)|(^(0){1}$)|([0-9].0-9?$)/,-->
<!--        pattern: /([1-9]([0-9]+)?(.[0-9]{1,2})?$)|(^(0){1}$)|([0-9].0-9?$)/,-->
<!--        message: '请输入正确的价格' }]"-->
<!--    >-->
<!--      <a-input v-model:value="book.price" />-->
<!--    </a-form-item>-->
<!--    <a-form-item-->
<!--        label="出版社"-->
<!--        name="publishName"-->
<!--        :rules="[{ required: true, message: '请输入出版社' }]"-->
<!--    >-->
<!--      <a-input v-model:value="book.publishName" />-->
<!--    </a-form-item>-->

<!--    <a-form-item :wrapper-col="{ offset: 8, span: 8 }">-->
<!--      <a-button type="primary" html-type="submit" @click="save()">保存</a-button>-->
<!--      <a-button style="margin-left: 50px" type="primary" ghost @click="reset()">重置</a-button>-->
<!--      <router-link to="/">-->
<!--        <a-button style="margin-left: 50px" >返回</a-button>-->
<!--      </router-link>-->
<!--    </a-form-item>-->

<!--  </a-form>-->
<!--</template>-->
<!--<script lang="ts">-->

<!--import {defineComponent, onMounted, reactive, ref} from 'vue';-->
<!--import {message, SelectProps } from "ant-design-vue";-->
<!--import axios from "axios";-->
<!--import { useRoute } from 'vue-router';-->
<!--import moment from 'moment';-->
<!--// import {useRouter} from "vue-router";-->


<!--export default defineComponent({-->
<!--  name: 'UpdateBook',-->
<!--  setup(){-->

<!--    const book = ref({});-->
<!--    const route = useRoute();-->
<!--    const data = JSON.parse(route.query.data+"");-->
<!--    //转换时间-->
<!--    data.publishTime = moment(data.publishTime);-->
<!--    //格式化时间-->
<!--    data.selfTime = moment(data.selfTime).format('YYYY-MM-DD');-->
<!--    book.value=data;-->
<!--    //分页-->
<!--    const pagination = ref({-->
<!--      current:1,-->
<!--      pageSize:5,-->
<!--      total:0-->

<!--    });-->


<!--    const category = ref<SelectProps['options']>([]);-->
<!--    const categoryId = ref();-->

<!--    const queryBookCategoryList = ()=>{-->
<!--      axios.get("http://localhost:8080/getBookListCategory").then(function (resp){-->
<!--        const data = resp.data;-->
<!--        category.value = data.map((item: {id: number, name: string}) => ({-->
<!--          value: item.id ,-->
<!--          label: item.name-->
<!--        }));-->
<!--      })-->
<!--    }-->
<!--    //重置函数-->
<!--    const reset = ()=>{-->
<!--      book.value = {};-->
<!--    }-->


<!--    //修改函数-->
<!--    // const updateBook = (book:any)=>{-->
<!--    //   router.push({-->
<!--    //     path:'/updateBook',-->
<!--    //     query:{data:JSON.stringify(book)}-->
<!--    //   })-->
<!--    // }-->
<!--    //保存函数-->
<!--    const save = ()=>{-->
<!--      //校验book内容是否为空-->
<!--      if (!(JSON.stringify(book.value)=='{}')){-->
<!--        axios.post("http://localhost:8080/saveOrUpdate",book.value).then((resp)=>{-->
<!--          message.success(resp.data);-->
<!--          book.value = {};-->
<!--        })-->
<!--      }-->
<!--    }-->

<!--    onMounted(()=>{-->
<!--      queryBookCategoryList();-->
<!--    })-->

<!--    return{-->
<!--      pagination,-->
<!--      // router,-->
<!--      save,-->
<!--      book,-->
<!--      category,-->
<!--      categoryId,-->
<!--      reset,-->
<!--      // updateBook-->
<!--    }-->
<!--  }-->
<!--});-->
<!--</script>-->


<template>
  <a-form
      :model="book"
      name="basic"
      :label-col="{ span: 8 }"
      :wrapper-col="{ span: 8 }"
      autocomplete="off"
  >
    <a-form-item
        label="图书类型"
        name="categoryId"
        :rules="[{ required: true, message: '请选择图书类型' }]"
    >
      <a-select
          v-model:value="book.categoryId"
          style="width: 510px;text-align: left;"
          :options="category"
      >
      </a-select>
    </a-form-item>

    <a-form-item
        label="图书名称"
        name="name"
        :rules="[{ required: true, message: '请输入图书名称' }]"
    >
      <a-input v-model:value="book.name" />
    </a-form-item>

    <a-form-item
        label="出版日期"
        name="publishTime"
        :rules="[{ required: true, message: '请输入出版日期' }]"
    >
      <a-date-picker v-model:value="book.publishTime" style="width: 510px"/>
    </a-form-item>

    <a-form-item
        label="价格"
        name="price"
        :rules="[{ required: true,
        pattern: /([1-9]([0-9]+)?(.[0-9]{1,2})?$)|(^(0){1}$)|([0-9].0-9?$)/,
        message: '请输入正确的价格' }]"
    >
      <a-input v-model:value="book.price" />
    </a-form-item>
    <a-form-item
        label="出版社"
        name="publishName"
        :rules="[{ required: true, message: '请输入出版社' }]"
    >
      <a-input v-model:value="book.publishName" />
    </a-form-item>

    <a-form-item
        label="上架时间"
        name="selfTime"
    >
      <a-input v-model:value="book.selfTime" disabled/>
    </a-form-item>

    <a-form-item :wrapper-col="{ offset: 8, span: 8 }">
      <a-button type="primary" html-type="submit" @click="save()">保存</a-button>
      <router-link to="/">
        <a-button style="margin-left: 50px" danger ghost @click="deleteBook(book.id)">删除</a-button>
      </router-link>
      <router-link to="/">
        <a-button style="margin-left: 50px" >返回</a-button>
      </router-link>
    </a-form-item>

  </a-form>
</template>
<script lang="ts">
import {defineComponent, onMounted, reactive, ref,computed} from 'vue';
import {message, SelectProps} from "ant-design-vue";
import axios from "axios";
import {useRoute} from "vue-router";
import moment from "moment";


export default defineComponent({
  name: 'UpdateBook',
  setup() {
    const router = useRoute();
    //这里需要转为字符串负责转换为对象会报错
    const data = JSON.parse(router.query.data+"")
    const book = ref({});
    //我们传递过来的时间，antdv的日期选择器无法使用，需要使用moment转换一下才能使用，不然报错
    data.publishTime = moment(data.publishTime);
    //格式化一下出版日期的时间
    data.selfTime = moment(data.selfTime).format('YYYY-MM-DD');
    book.value = data;

    const category = ref<SelectProps['options']>([]);
    const categoryId = ref();

    const queryBookCategoryList = ()=>{
      axios.get("http://localhost:8080/getBookListCategory").then(function (resp){
        const data = resp.data;
        category.value = data.map((item: {id: number, name: string}) => ({
          value: item.id ,
          label: item.name
        }));
      })

    }

    const save = ()=>{
      //校验book内容是否为空，触发组件的非空校验
      if (!(JSON.stringify(book.value)== '{}')){
        axios.post("http://localhost:8080/saveOrUpdate",book.value).then((resp)=>{
          message.success(resp.data);
        })
      }
    }

    const deleteBook = (id:number)=>{
      axios.get("http://localhost:8080/deleteBook/"+id).then((resp)=>{
        message.success(resp.data);
      })
    }

    onMounted(()=>{
      queryBookCategoryList();

    })

    return {
      book,
      category,
      categoryId,
      save,
      deleteBook,
    };
  },
});
</script>