<template>
  <div >


<div style="margin-top: 15px; max-width:600px" >

  <el-input placeholder="Please input" v-model="organizations.searchQuery" @input="organizationsSearchRequest" class="input-with-select">
    <el-select v-model="organizations.searchBy" slot="prepend" placeholder="Select" class="organizations__search-select">
  
  <el-option v-for="option in organizations.searchByOptions"  :key="option.value" :label="option.label" :value="option.value"></el-option>

  </el-select>

  </el-input>



<el-card>

  <div  v-show="organizations.loading">
    <i class="el-icon-loading"></i> Поиск...
  </div>

  <div v-if="!organizations.loading && organizations.searchResultArr.length > 0">
  
    <el-button  v-for="item in organizations.searchResultArr" :key="item.id" @click="organizationsAddHandle(item.id)"> 
      <span> {{item.name}}</span> <span>  {{item.id }} {{item.checked}}</span> 
    </el-button>
  </div >

  <p v-else>
    Нет данных
  </p>

 
</el-card>
{{
  organizations.selectedArr
}}

  <div v-for="item in organizations.selectedArr" :key="item.id" >
    <el-tag @close="abc(item)" closable  class="text item"    :disable-transitions="false"> 
      <span> {{item.name}}</span> <span>  {{item.id }} {{item.checked}}</span> 
    </el-tag>
  </div>


</div>


  </div>
</template>

<script>
  export default {
    data() {
      return {
        organizations: {
                loading: false,
                selectedArr: [], // список выбранных организаций 
                searchResultArr: [], 
                searchBy:'name',
                searchQuery:'',
                searchByOptions: [{value:'name' , label :"Название"}, {value:'id' , label :"ID"} ],
                selectAll: false,
            },

      };
    },

    methods: {
     async organizationsSearchRequest () {
      

      try {
        this.organizations.loading = true;
        this.organizations.searchResultArr = []
        let res = await new Promise((resolve) => {setTimeout(() => {resolve([{id: 32, external_id: null, name: "ПАО АвтоСантехТяжБанк"},{id: 33, external_id: null, name: "ООО Компания ОмскСантехМетал"}]);}, 1000);});
        
        

          this.organizations.searchResultArr = res.map((item)=>{
            
            item.checked = this.selectedArr.some(element => item.id === element.id)

            return item

          }) 
          
        

      }catch (err){
          console.log(err)
      }finally {
        this.organizations.loading = false;
      }

      },

      handleClose(item) {
        console.log(item);
      },

      organizationsAddHandle(id){
       
        const targetIndex = this.organizations.searchResultArr.findIndex((item)=>{
             return item.id === id
        })

        if ( targetIndex != -1  ) {
          this.organizations.selectedArr = [...this.organizations.selectedArr, ...this.organizations.searchResultArr.splice(targetIndex,1) ]
        }
      
      }
    },
    mounted() {

    }
  
  };
</script>


<style >
.organizations__search-select{
  min-width: 120px;
}
</style>
