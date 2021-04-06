<template>
 <div class="user-profile">
   <div class="user-profile__user-panel">
      <h1 class="user-profile__username">{{user.username }}</h1>
        <div class="user-profile__admin-badge" v-if="user.isAdmin">
          Admin
        </div>
        
        <div class="user-profile__follower-count">
          <strong>Followers: </strong> {{followers }}
        </div>
        <form class="user-profile__create-twoot" @submit.prevent="createNewTwoot" :class="{'--exceeded': NewTwootCharacterCount > 180}">
        <label for="newTwoot"><strong>New Twoot:</strong>({{NewTwootCharacterCount}}/180)</label>
        <textarea id="newTwoot" rows="4" v-model="newTwootContent" />    

        <div class="user-profile__create-twoot-type" >
          <label for="newTwootType"><strong>Type: </strong></label>
          <select id="newTwootType">
            <option :value="option.value" v-for="(option,index) in twootTypes" :key="index">
              {{ option.name}}
            </option>
          </select>
        </div>
        <button>
          Twoot it!
        </button>

        </form>
   </div>


  <div class="user-profile__twoots-wrapper">

      <TwootItem 
      v-for="twoot in user.twoots" 
      :key="twoot.id" 
      :username="user.username"
       :twoot="twoot" />
    
  </div>

 </div>
</template>

<script>

import TwootItem from "./TwootItem";

export default {
  name: 'HelloWorld',
  components:{TwootItem},
  data(){
    return{
      newTwootContent: '',
      selectedTwootType: 'instant',
      twootTypes: [
        { value: 'draft', name: 'Draft'},
        { value: 'instant', name: 'Instant twoot'}
      ],
      followers: 0,
      user: {
        id: 1,
        username: 'MarianM',
        firstName: 'Marian',
        lastName: 'Marin',
        email: 'marin_puiu99@yahoo.com',
        isAdmin: true,
        twoots:[
          { id: 1, content: 'Twotter is Amazing!'},
          { id: 2, content: 'Milsugi cu lapte'}
        ]
      }
    }
  },

  watch:{

    followers(newFollowerCount, oldFollowerCount){

      if(oldFollowerCount < newFollowerCount){
        console.log(`${this.user.username} has gained a follower!`)
      }

    }

  },

  computed:{
   NewTwootCharacterCount(){
     return this.newTwootContent.length;

   }
  },

  methods: {
    followUser(){
      this.followers++
    },

    createNewTwoot(){
      if (this.newTwootContent && this.selectedTwootType !== 'draft'){
        this.user.twoots.unshift({
          id: this.user.twoots.length +1,
          content: this.newTwootContent
        })
      }
      this.newTwootContent = '';
    }
  },

  mounted() {
    this.followUser();
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style  lang="scss" scoped>

.user-profile {
  display: grid;
  grid-template-columns: 1fr 3fr;
  grid-gap: 50px;
  padding: 50px 5%;
  .user-profile__user-panel {
    display: flex;
    flex-direction: column;
    padding: 20px;
    background-color: white;
    border-radius: 5px;
    border: 1px solid #DFE3E8;
    margin-bottom: auto;
    h1 {
      margin: 0;
    }
    .user-profile__admin-badge {
      background: rebeccapurple;
      color: white;
      border-radius: 5px;
      padding: 0 10px;
      font-weight: bold;
    }
  }
  .user-profile__twoots-wrapper {
    display: grid;
    grid-gap: 10px;
    margin-bottom: auto;
  }
}

</style>
