<template>
<div class="wrapper">
  <div>

    <div class='wrapper'>
      <div class='grid'>
        <div>
          <h3 class='post-item'>{{tweet.username}}</h3>
          <div class='post-item'>
            <img class='avatar post-item' :src="tweet.picture">
          </div>
          <div class='micro-gap'></div>
          <p class='post-item'>{{tweet.text}}</p>
          <hr>
        </div>
      </div>
    </div>

    <h3 class='page-item'>Comments</h3>

    <div class="small-gap"></div>
    <div class='wrapper'>
      <form class='grid' v-on:submit.prevent="addComment">
        <input class='input-item' v-model="addedName" placeholder="Username">
        <textarea class='input-item' v-model="addedComment" placeholder="Write something"></textarea>
        <button class='input-item btn btn-warning' type="submit">Add Comment</button>
      </form>
    </div>

    <div class='small-gap'></div>

    <div class='wrapper'>
      <div class='grid'>
        <div v-for="comment in tweet.comments.slice().reverse()" :key="comment.id">
          <h3 class='post-item'>{{comment.username}}</h3>
          <div class='post-item'>
            <img class='avatar post-item' :src="comment.picture">
          </div>
          <div class='micro-gap'></div>
          <div class='post-item' v-if="comment.owned">
            <button class='post-item' v-on:click="del(tweet, comment)">Delete</button> 
            <button class='post-item' v-if="comment.editing" v-on:click="edit(comment)">Done</button>
            <button class='post-item' v-else v-on:click="edit(comment)">Edit</button>
          </div>
          <p class='post-item' v-if="!comment.editing">{{comment.text}}</p>
          <textarea class='input-item' v-else v-model="comment.text"></textarea>

          <hr>
        </div>
      </div>
    </div>

  </div>


</div>
</template>

<script>
export default {
  name: 'CommentList',
  data() {
    return {
      addedName: '',
      addedComment: ''
    }
  },
  props: {
    tweet: Object,
  },
  methods: {

    del(tweet, comment) {
      tweet.comments.splice(tweet.comments.indexOf(comment), 1);
      //this.$root.$data.tweets.indexOf(tweet).comments.splice(this.$root.$data.tweets.indexOf(tweet),1);
    },
    edit(comment) {
      if (comment.editing)
        comment.editing = false;
      else
        comment.editing = true;
    },
    addComment() {
       // if (!(this.number in this.comments))
         //   Vue.set(app.comments, this.number, new Array);
        let comments = this.$props.tweet.comments; //this.$root.$data.comments[tweet.id-1];

        //let picture = mock[0].picture;
        //let picture = comments[Math.floor(Math.random()*comments.length)].picture;
        let picture = this.$root.$data.tweets[0].picture;
        let id = 1 ;
        if (comments.length > 0)
          id = comments[comments.length-1].id + 1;
        comments.push({
            id: id,
            username: this.addedName,
            text: this.addedComment,
            editing: false,
            owned: true,
            picture: picture
            //time: moment().format('MMMM Do YYYY, h:mm:ss a')
        });
        this.addedName = '';
        this.addedComment = '';
    },

    //replyPage(comment) {

      //this.$root.$data.cart.push(comment);
//    },
  },
}
</script>

<style scoped>

</style>