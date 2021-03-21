<template>
<div class="wrapper">
  <div>

    <h3 class='page-item'>Discussions</h3>

    <div class="small-gap"></div>
    <div class='wrapper'>
      <form class='grid' v-on:submit.prevent="addComment">
        <input class='input-item' v-model="addedName" placeholder="Username">
        <textarea class='input-item' v-model="addedComment" placeholder="Write something"></textarea>
        <button class='input-item btn btn-warning' type="submit">Post</button>
      </form>
    </div>

    <div class='small-gap'></div>

    <div class='wrapper'>
      <div class='grid'>
        <div v-for="tweet in tweets.slice().reverse()" :key="tweet.id">
          <h3 class='post-item'>{{tweet.username}}</h3>
          <div class='post-item'>
            <img class='avatar post-item' :src="tweet.picture">
          </div>
          <div class='micro-gap'></div>
          <div class='post-item' v-if="tweet.owned">
            <button class='post-item' v-on:click="del(tweet)">Delete</button> 
            <button class='post-item' v-if="tweet.editing" v-on:click="edit(tweet)">Done</button>
            <button class='post-item' v-else v-on:click="edit(tweet)">Edit</button>
          </div>
          <p class='post-item' v-if="!tweet.editing">{{tweet.text}}</p>
          <textarea class='input-item' v-else v-model="tweet.text"></textarea>

          <router-link class='post-item' v-if="tweet.comments" :to="'/tweet/' + tweet.id">
            <h5 class='post-item'>Reply</h5>
          </router-link>
          <hr>
        </div>
      </div>
    </div>

  </div>


</div>
</template>

<script>
export default {
  name: 'TweetList',
  data() {
    return {
      addedName: '',
      addedComment: ''
    }
  },
  props: {
    tweets: Array
  },
  methods: {

    del(tweet) {
      this.$root.$data.tweets.splice(this.$root.$data.tweets.indexOf(tweet),1);
    },

    edit(tweet) {
      if (tweet.editing)
        tweet.editing = false;
      else
        tweet.editing = true;
    },

    addComment() {
       // if (!(this.number in this.comments))
         //   Vue.set(app.comments, this.number, new Array);
        let tweets = this.$root.$data.tweets;

        //let picture = mock[0].picture;
        //let picture = tweets[Math.floor(Math.random()*tweets.length)].picture;
        let picture = tweets[0].picture;
        tweets.push({
            id: tweets[tweets.length-1].id + 1,
            username: this.addedName,
            text: this.addedComment,
            picture: picture,
            editing: false,
            owned: true,
            comments: []
            //time: moment().format('MMMM Do YYYY, h:mm:ss a')
        });
        this.addedName = '';
        this.addedComment = '';
    },

    //replyPage(tweet) {

      //this.$root.$data.cart.push(tweet);
//    },
  },
}
</script>

<style scoped>

</style>