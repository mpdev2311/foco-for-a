<template>
<div class="containe grid-lg my-2 py-2">
<div class="cardi mb-2" v-if="listenQuotes.length > 0">
  <div class="carde-header">
    <div class="h4">Acompanhando</div>
  </div>
   <div class="cardi-bady">
     <WatchListQuotes :listen-quotes="listenQuotes" @unlist="onUnlisten" />
   </div>
</div>

 <div class="cardi">
   <div class="cardi-header">
     <div class="h4">Todas as moedas</div>
   </div>
   <div class="cardi-bady">
    <ListQuotes
     :quotes="quotes" 
     :listen-quotes="listenQuotes"
     @listen="onListen"
     @unlisten="onUnlisten"
     />
   </div>
 </div>
</div>  
</template>

<script>
import { onMounted, reactive, toRefs  } from 'vue';
import api from './services/api'
import ListQuotes from './components/ListQuotes.vue';
import WatchListQuotes from "./components/WatchListQuotes.vue";
export default {
   name: 'App',
   components: {ListQuotes, WatchListQuotes },
   setup() { 
     
     const data = reactive({
       quotes: {},  
       listenQuotes: [],       
     });
     
     onMounted(async () => {
       const response = await api.all();
       console.log(response);
       data.quotes = response.data;
        console.log(data);
     })
     
     function onListen(code) {
       data.listenQuotes.push(code)
     }

     function onUnlisten(code) {
       data.listenQuotes = data.listenQuotes.filter(key => key !== code )
     }

     return {
        ...toRefs(data),
        onListen,
        onUnlisten, 
     };   
   },
 };
</script>


