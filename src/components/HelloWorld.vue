<template>
  <div >

<el-form >

  <el-form-item size="mini"
                error="выы"
  >

    <el-checkbox v-model="organizations.selectAll"> Выбрать все организации
      <el-tooltip class="item" effect="dark" content="Top Left prompts info" placement="top-start">
        <i class="el-icon-info" />
    </el-tooltip> </el-checkbox>
  </el-form-item>
  <el-form-item
  >


  <el-input :disabled="organizations.selectAll" placeholder="Поиск организаций" v-model.trim="organizations.searchQuery" @input="organizationsSearchRequest" class="organizations__search-input" >
    <el-select :disabled="organizations.selectAll" v-model="organizations.searchBy" slot="prepend" placeholder="Select" >

  <el-option v-for="option in organizations.searchByOptions"  :key="option.value" :label="option.label" :value="option.value"></el-option>

  </el-select>

  </el-input>


<el-card class="organizations__search-result" v-show="organizations.searchQuery">

  <div  v-if="organizations.loading">
    <i class="el-icon-loading"></i> Поиск...
  </div>

  <div v-else>
      <p  v-show="!organizations.searchResultArr.length">
        Нет данных
    </p>

    <el-tag :type="item.checked ? '' : 'info' "  disable-transitions  v-for="item in organizations.searchResultArr" :key="item.id" @click="organizationsCheckHandle(item)">
       <span> <i class="el-icon-check"  :class="{ hidden:!item.checked }"/> {{item.name}}</span> <span> ID: {{item.id }}</span>
    </el-tag>

  </div >


</el-card>

    <div class="organizations__selected" v-show="!organizations.selectAll">
      <el-tag v-for="item in organizations.selectedArr"  :key="item.id" @close="organizationsCheckHandle(item)" closable  class="text item"    :disable-transitions="false">
        <span> {{item.name}}</span> <span> ID:  {{item.id }} </span>
      </el-tag>
    </div>

  </el-form-item>
</el-form>


  </div>
</template>

<script>
  export default {
    data() {
      return {
        organizations: {
                loading: false,
                selectedArr: [{id: 66633, checked:true,external_id: null, name: "ООО Компания ОмскСантехМетал"},{ "id": 32, "external_id": null, "name": "ПАО АвтоСантехТяжБанк", "checked": true }], // список выбранных организаций
                searchResultArr: [],
                searchBy:'name',
                searchQuery:'ss',
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
        let res = await new Promise((resolve) => {setTimeout(() => {resolve([{id: 32, external_id: null, name: "ПАО АвтоСантехТяжБанк"},{id: 33, external_id: null, name: "ООО Компания ОмскСантехМетал"},{id: 55, external_id: null, name: "fhdfjj fddg gf gd"}]);}, 100);});
        const selectedIdArr = this.organizations.selectedArr.map((item)=>{
          return item.id
        })
        this.organizations.searchResultArr = res.map((item)=>{

            item.checked =  selectedIdArr.includes(item.id)

            return item

          })


      }catch (err){
          console.log(err)
      }finally {
        this.organizations.loading = false;
      }

      },

      organizationsCheckHandle(item){

if(!Object.keys(item).length){
  return;
}

        const selectedArrTargetIndex = this.organizations.selectedArr.findIndex((selectedItem)=>{
          return selectedItem.id === item.id
        })

        const searchArrTargetIndex = this.organizations.searchResultArr.findIndex((selectedItem)=>{
          return selectedItem.id === item.id
        })


if (!item.checked) {
  this.organizations.searchResultArr[searchArrTargetIndex].checked = true;
  this.organizations.selectedArr.unshift(this.organizations.searchResultArr[searchArrTargetIndex])
  return;
}

   if(selectedArrTargetIndex > -1 ){
     this.organizations.selectedArr.splice(selectedArrTargetIndex,1)
   }
   if(searchArrTargetIndex > -1 ){
          this.organizations.searchResultArr[searchArrTargetIndex].checked = false
   }
      }
    },

    mounted() {
      this.organizationsSearchRequest ()
    }

  };
</script>


<style  lang="scss">
.organizations
{

  &__search-result{
    margin-bottom: 1.5rem;
    & .el-card__body{
      padding: 0 ;
    }
    & .el-tag{
      width: 100%;
        display: flex;
        justify-content: space-between;

    }
    .el-icon-check.hidden {
      opacity: 0;
    }
  }

  &__selected {
    display: flex;
    flex-wrap: wrap;
  }

  &__search-input{

    & .el-select{
      min-width: 120px;
    }

  }

}

</style>
