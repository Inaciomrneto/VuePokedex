<template>  
  <v-chip :color=getColor(typePoke)>{{typePoke.charAt(0).toUpperCase() + typePoke.slice(1)}}</v-chip>
</template>

<script>  

export default {
  name: 'LabelTypeColor',

  components: {
  },

  props:{
    typePoke: String,
  },

methods:{
  getColor(type){
    if(type === 'normal'){
      return '#b3b3a8';
    }
    else if(type === 'poison'){
      return '#a85e9a';
    }
    else if(type === 'psychic'){
      return '#ea62a8';
    }
    else if(type === 'grass'){
      return '#82c963';
    }
    else if(type === 'ground'){
      return '#dbbf6b';
    }
    else if(type === 'ice'){
      return '#87dffb';
    }
    else if(type === 'fire'){
      return '#ee524b';
    }
    else if(type === 'rock'){
      return '#c1b176';
    }
    else if(type === 'dragon'){
      return '#8577f5';
    }
    else if(type === 'water'){
      return '#50a6f9';
    }
    else if(type === 'bug'){
      return '#b7c54c';
    }
    else if(type === 'dark'){
      return '#8b6958';
    }
    else if(type === 'fighting'){
      return '#ba5e55';
    }
    else if(type === 'ghost'){
      return '#7675c1';
    }
    else if(type === 'steel'){
      return '#b8b7c9';
    }
    else if(type === 'flying'){
      return '#719cee';
    }
    else if(type === 'electric'){
      return '#f3d55e';
    }
    else if(type === 'fairy'){
      return '#e7a5e7';
    }
  },
},
};
</script>