<template>
 <div class="pl-[78px] mt-[28px] pr-[77px] font-['Roboto'] ">
    
    <form class="w-[231px] md:w-[231px] lg:w-[431px] 2xl:w-[631px] " >
  <div class="flex items-center bg-[#5A5C66] py-2">
    <input  v-model="search " class="appearance-none bg-[#5A5C66] border-none w-full pr-[10px] text-white mr-3 py-1 px-2 leading-tight focus:outline-none" type="text" placeholder="Поиск " >
    <button class="flex-shrink-0   text-sm  text-white py-1 px-4 rounded" type="button">
     <img :src="mySVG" class="" />
    </button>
   
  </div>
</form>

 <div class=" mt-[20px] w-[470px] md:w-[600px] lg:w-[972px] 2xl:w-[1077px]    ">
  <div class="flex px-[8px] py-[19px] h-[54px] w-[470px] md:w-[600px] lg:w-[972px] 2xl:w-[1077px] bg-[#474955] drop-shadow font-semibold leading-[19.28px] " >
   <span class="cursor-pointer text-white " >ID</span>
    <img src="../assets/Group1.png" alt="arrow" @click="sortBy('title')" class="w-[11px] h-[5px] xl:mt-[10px] xl:ml-[24px] m-2 cursor-pointer text-white "  >
   <span class="xl:pl-[151.3px]  pl-[80px] text-[14px] cursor-pointer text-white" @click="sortBy('title')">Заголовок</span>
    <img src="../assets/Group1.png" alt="arrow" class="w-[11px] h-[5px] mt-[10px] ml-[24px] lg:ml-[50px] cursor-pointer text-white" @click="sortBy('title')" > 
   <span class="xl:pl-[410px] lg:pl-[360px] md:pl-[170px] pl-[82px] cursor-pointer text-white " @click="sortBy('body') ">Описание</span>
   <img src="../assets/Group1.png" alt="arrow" class="w-[11px] h-[5px] mt-[10px] cursor-pointer md:ml-[60px] ml-[40px] text-white" @click="sortBy('title')" >
   </div>
  <div class="flex border-px  items-center text-[13px] leading-[19.28px]  font-[138%]" v-for="row in  resultQuery" :key="row.id" >
   <div class="w-[110px] h-[87px] border-r border-[1px]    text-center pt-[41px]">{{ row.id }}</div>
   <div class="w-[529px] h-[87px] border-r  border-b-none border-[1px] text-center  pt-[41px] ">{{ row.title }}</div>
   <div class="w-[438px] h-[87px] border-b-none border-r border-[1px] pl-[11px] pt-[15px] ">{{ row.body }}</div>

  </div>
 </div>


  
    <div class=" flex w-full mt-[26px] mb-[12px] justify-items-start font-['Roboto']  font-[500]  border-gray-500">
              <button class="xl:pl-[41px] xl:text-[24px]" @click="onClickFirstPage" >Назад</button>
              <div class="flex lg:pl-[200px] xl:pl-[375px] pl-4">
              <div v-for="page in pages" :key="page" class=" w-auto pl-2 text-[18px]  italic font-[700]     hover:bg-gray-300 xl:pt-[5px] pl-[5px] cursor-pointer" 
                 @click="pageClick(page)"
                 :class="{'text-[#7EBC3C]':page === pageNumber}">
                 {{ page }}

              </div>
              </div>
              <button class="w-[20px] pl-4 xl:pl-[232px] xl:text-[24px] lg:pl-[200px] "  @click="onClickLastPage" >
             Далее</button>
      </div> 
 
 </div>
</template>

<script>



export default {
    name: "v-table",
    props: {
        users_data: {
            type: Array,
            default: () => {
                return [];
            }
        }
    },
    data() {
        return {
         search: null,
         usersPerPage:10,
         pageNumber:1,
         mySVG: require('../assets/Search.png'),
         mySVG1: require('../assets/Group1.png'),
        

           
        };
    },
    created(){
     
    },
   computed:{
     resultQuery:function() {
      if (this.search !== null) {
      const a=this.paginatedUsers.filter(post => post.title.toLowerCase().includes(this.search.toLowerCase()));
      if(a){
       return a
      }
      
      
     
      
      
      } else {
       
    let from = (this.pageNumber -1) * this.usersPerPage;
    let to =from + this.usersPerPage;
    return this.users_data.slice(from,to);
      }
      
     },
     
    filteredUsers(){
     return this.users_data.filter(post =>
        post.body.toLowerCase().includes(this.search.toLowerCase()))
   },
  
   
    pages(){
    return Math.ceil(this.users_data.length /10);
    
  },
  
  paginatedUsers: function(){

    let from = (this.pageNumber -1) * this.usersPerPage;
    let to =from + this.usersPerPage;
    return this.users_data.slice(from,to);
   
  }
   
  
 },
  methods:{
    onClickLastPage(){
      this.pageNumber =this.pageNumber +1
   
      if(this.pageNumber >9){
        return this.pageNumber =10

      }
    },
     onClickFirstPage(){
      this.pageNumber =this.pageNumber -1
      if(this.pageNumber <2){
        return this.pageNumber=1

      }
    },

  
  
 pageClick(page){
   this.pageNumber = page
 this.router.push({  params: { username: pageNumber } })
},
sortBy(prop){
const sort =  this.users_data.sort((a,b) =>a[prop] > b[prop] ? -1 : -1);
  
 sort();
},



   
  },
    components: {  }
}
</script>

