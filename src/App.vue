<template>
  <div id="app" class="flex container h-screen w-full">
    <div class="lg:w-1/5 border-r border-lighter lpx-2 lg:px-6 py-2 flex flex-col justify-between">
      <div>
          <button class="h-12 w-12 hover:bg-lightblue text-3xl rounded-full text-blue">
            <i class="fab fa-twitter"></i>
          </button>
          <div>
            <button v-for="tab in tabs" :key="tab.id" @click="id = tab.id" :class="`focus:outline-none hover:text-blue flex items-center py-2 px-4 hover:bg-lightblue rounded-full mr-auto mb-3 ${ id === tab.id ? 'text-blue' : ''}`">
              <i :class="`${ tab.icon } text-2xl mr-4 text-left`"></i>
              <p class="text-lg font-semibold text-left hidden lg:block"> {{ tab.title }} </p>
            </button>
          </div>
          <button class="text-white bg-blue rounded-full font-semibold focus:outline-none w-12 h-12 lg:h-auto lg:w-11/12 p-4 hover:bg-darkblue">
            <p class="hidden lg:block"> Tweeter </p>
            <i class="fas fa-plus lg:hidden"></i>
          </button>
      </div>
        
      <div class="lg:w-full relative">

        <button @click="dropdown = true" class="flex items-center w-full hover:bg-lightblue rounded-full p-2 focus:outline-none">
          <img src="../public/img/valentin.png" class="w-10 h-10 rounded-full border-lighter" />
          <div class="hidden lg:block ml-4">
            <p class="text-sm font-bold leading-tight"> Valentin </p>
            <p class="text-sm leading-tight"> @VAal35 </p>
          </div>
          <i class="hidden lg:block ml-4 fas fa-ellipsis-h ml-auto text-lg"></i>
        </button>

        <div v-if="dropdown === true" class="absolute bottom-0 left-0 w-64 rounded-lg shadow-md border-lightest bg-white mb-16">
          <button @click="dropdown = false" class="p-3 flex items-center w-full hover:bg-lightest p-2 focus:outline-none">
            <img src="../public/img/valentin.png" class="w-10 h-10 rounded-full " />
            <div class="ml-4">
              <p class="text-sm font-bold leading-tight"> Valentin </p>
              <p class="text-sm leading-tight"> @VAal35 </p>
            </div>
            <i class="fas fa-ellipsis-h ml-auto test-blue"></i>
          </button>
          <button @click="dropdown = false" class="w-full text-left hover:bg-lightest border-t border-lighter p-3 test-sm focus:outline-none">
            Ajouter un compte existant
          </button>
          <button @click="dropdown = false" class="w-full text-left hover:bg-lightest border-t border-lighter p-3 test-sm focus:outline-none">
            Se déconnecter de @VAal35
          </button>
        </div>
      </div>
    </div>

    <!-- section tweet -->
    
    <div class="w-full md:w-1/2 h-full overflow-y-scroll">
      <div class="px-5 py-3 border-b border-lighter flex items-center justify-between">
        <h1 class="text-xl font-bold"> Accueil </h1>
        <i class="far fa-star text-xl text-blue"></i>
      </div>

      <div class="px-5 py-3 border-b-2 border-lighter flex">
        <div>
          <img src="../public/img/valentin.png" class="flex-none w-14 h-13 rounded-full border border-lighter"/>
        </div>
          <form v-on:submit.prevent = "addNewTweet" class="w-full px-4 relative">
          <textarea v-model="tweet.content" placeholder="Quoi de neuf ?" class="mt-3 pb-3 w-full focus:outline-none"/>
          <div class="flex items-center">
            <i class="text-lg text-blue mr-4 fa-solid fa-gif"></i>
            <i class="text-lg text-blue mr-4 fa-solid fa-film"></i>
            <i class="text-lg text-blue mr-4 far fa-chart-bar"></i>
            <i class="text-lg text-blue mr-4 far fa-smile"></i>
          </div>
          <button type="submit" class="h-10 px-4 text-white font-semibold bg-blue hover:bg-darkblue focus:outline-none rounded-full absolute bottom-0 right-0">
            Tweeter
          </button>
        </form>
      </div>

      <div class="flex flex-col-reverse">
        <div v-for="tweet in tweets" :key="tweet.id" class="w-full p-4 border-b hover:bg-lighter flex">
          <div class="flex-none mr-4">
            <img src="../public/img/valentin.png" class="h-12 w-12 rounded-full flex-none"/>
          </div>
          <div class="w-full">
            <div class="flex items-center w-full">
              <p class="font-semibold">Valentin Dauvier </p>
              <p class="text-sm text-dark ml-2"> VAal35 </p>
              <p class="text-sm text-dark ml-2"> 1 sec </p>
              <i class="fas fa-ellipsis-h text-dark ml-auto"></i>
            </div>
            <p class="py-2">
              {{ tweet.content }}
            </p>
            <div class="flex items-center justify-between w-full">
              <div class="flex items-center text-sm text-dark">
                <i class="far fa-comment mr-3"></i>
                <p> 0 </p>
              </div>
              <div class="flex items-center text-sm text-dark">
                <i class="fas fa-retweet mr-3"></i>
                <p> 0 </p>
              </div>
              <div class="flex items-center text-sm text-dark">
                <i class="fas fa-heart mr-3"></i>
                <p> 1 </p>
              </div>
              <div class="flex items-center text-sm text-dark">
                <i class="fas fa-share-square mr-3"></i>
              </div>
            </div>
          </div>
        </div>
      </div>

      <div v-for="follow in following" :key="follow.id" class="w-full p-4 border-b hover:bg-lighter flex">
        <div class="flex-none mr-4">
          <img :src="`${follow.src}`" class="h-12 w-12 rounded-full flex-none"/>
        </div>
        <div class="w-full">
          <div class="flex items-center w-full">
            <p class="font-semibold"> {{ follow.name }} </p>
            <p class="text-sm text-dark ml-2"> {{ follow.handle }} </p>
            <p class="text-sm text-dark ml-2"> {{ follow.time }} </p>
            <i class="fas fa-ellipsis-h text-dark ml-auto"></i>
          </div>
          <p class="py-2">
            {{ follow.tweet }}
          </p>
          <div class="flex items-center justify-between w-full">
            <div class="flex items-center text-sm text-dark">
              <i class="far fa-comment mr-3"></i>
              <p> {{ follow.comments }} </p>
            </div>
            <div class="flex items-center text-sm text-dark">
              <i class="fas fa-retweet mr-3"></i>
              <p> {{ follow.retweets }} </p>
            </div>
            <div class="flex items-center text-sm text-dark">
              <i class="fas fa-heart mr-3"></i>
              <p> {{ follow.like }} </p>
            </div>
            <div class="flex items-center text-sm text-dark">
              <i class="fas fa-share-square mr-3"></i>
            </div>
          </div>
        </div>
        </div>

    </div>
  

    <!-- section tendances -->

    <div class="md:block hidden w-1/3 h-full border-l border-lighter py-2 px-6 overflow-y-scroll relative">
      <input class="pl-12 rounded-full w-full p-2 bg-lighter text-sm" placeholder="Recherche Twitter" />
      <i class="fas fa-search absolute left-0 top-0 mt-4 ml-12 text-sm text-light"></i>
      <div class="w-full rounded-lg bg-lightest my-6">
        <div class="flex items-center justify-between p-3">
          <p class="text-lg font-bold"> Tendances pour vous </p>
          <i class="fas fa-cog text-lg text-blue"></i>
        </div>
        <button v-for="trend in trending" :key="trend.id" class="w-full flex justify-between hover:bg-lighter p-3 border-t border-lighter">
          <div>
            <p class="text-xs text-left leading-tight text-dark"> {{ trend.top}} </p>
            <p class="font-semibold text-sm text-left leading-tight"> {{ trend.title}} </p>
            <p class="text-left text-sm leading-tight text-dark"> {{ trend.bottom}} </p>
          </div>
          <i class="fas fa-ellipsis-h text-lg text-dark"></i>
        </button>
        <button class="p-3 w-full hover:bg-lighter text-left text-blue border-t border-lighter">
          Voir plus
        </button>
      </div>
      <div class="w-full rounded-lg bg-lightest my-4">
        <div class=" p-3">
          <p class="text-lg font-bold"> Suggestions </p>
        </div>
        <button v-for="friend in friends" :key="friend.id" class="w-full flex items-center hover:bg-lighter p-3 border-t border-lighter">
          <img :src="`${ friend.src }`" class="w-12 h-12 rounded-full border border-lighter" />
          <div class="hidden lg:block ml-4">
            <p class="text-sm font-bold leading-tight"> {{ friend.name }} </p>
            <p class="text-sm leading-tight"> {{ friend.handle }} </p>
          </div>
          <button class="ml-auto bg-black text-sm text-white py-1 px-4 rounded-full">
            Suivre
          </button>
        </button>
        <button class="p-3 w-full hover:bg-lighter text-left text-blue border-t border-lighter">
          Voir plus
        </button>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  name: 'App',
  data() {

    return {

      tabs: [
        {icon: 'fas fa-home', title: 'Accueil', id:'home'},
        {icon: 'fas fa-hashtag', title: 'Explorer', id: 'explore'},
        {icon: 'far fa-bell', title: 'Notifications', id: 'notifications'},
        {icon: 'far fa-envelope', title: 'Messages', id: 'messages'},
        {icon: 'far fa-bookmark', title: 'Signets', id: 'bookmarks'},
        {icon: 'fas fa-clipboard-list', title: 'Listes', id: 'lists'},
        {icon: 'far fa-user', title: 'Profil', id: 'profile'},
        {icon: 'fas fa-ellipsis-h', title: 'Plus', id: 'more'}
      ],
      id: 'home',
      dropdown: false,
      trending: [
        {top: 'Tendances en France', title: 'Benjamin', bottom: '245k tweets'},
        {top: 'Musique Tendances', title: 'Stromae', bottom: '135k Tweets'},
        {top: 'Sport Tendances', title: 'Stade Rennais', bottom: '40k tweets'},
        {top: 'Politique Tendaces', title: 'Jean Lassalle', bottom: '40k tweets'},
        {top: 'Tendances aux Etats Unis', title: 'Beyonce', bottom: '25.4k tweets'},
      ],
      friends: [
        {src: './img/musk.jpg', name: 'Elon Musk', handle: '@teslaBoy'},
        {src: './img/mozart.jpg', name: ' Wolf Mozart', handle: '@musique'},
        {src: './img/zidane.jpg', name: 'Zidane', handle: '@Zizzou'}
      ],
      following: [
        {src: './img/stromae.jpg', name: 'Stromae', handle: '@zizzou', time: '20 min', tweet: 'De retour sur scène !', comments: '1,000', retweets: '550', like: '10,003'},
        {src: './img/mbappe.jpg', name: 'Kylian Mbappé', handle: '@kylian17', time: '55 min', tweet: 'Merci pour tous vos messages après cette magnifique victoire', comments: '2,030', retweets: '50', like: '20,003'},
        {src: './img/gates.jpg', name: 'Bill Gates', handle: '@billGates3', time: '1.4 hr', tweet: " J'ai sous la main une idée révolutionnaire...", comments: '1,000', retweets: '1,000', like: '205,000'},
        {src: './img/macron.jpg', name: 'Emmanuel Macron', handle: '@teslaBoy', time: '1.4 hr', tweet: "N'oubliez pas votre devoir de citoyen. Allez voter dimanche !", comments: '1,005', retweets: '1,000', like: '105,000'}
      ],
      tweets: [
        {content: 'Vraiment cool Twitter !'}
      ],
      tweet: {content: ''}
    }
  },
  methods: {
    addNewTweet() {
      let newTweet = {
        content: this.tweet.content
      };
      this.tweets.push (newTweet)
    }
  }

}

</script>

