<template>
  <div class="user-profile">
    <div class="user-profile_user-panel">
      <h1 class="user-profile_username">@{{ user.username }}</h1>
      <div class="user-profile_admin-badge" v-if="user.isAdmin">Admin</div>
      <div class="user-profile_admin-badge" v-else>Regular Joe</div>
      <div class="userprofile_follower-count">
        <strong>Followers:</strong> {{ followers }}
      </div>
      <form class="user-profile_create-twoot" @submit.prevent="createNewTwoot" :class="{'--exceeded': newTwootCharacterCount > 180}">
        <label for="newTwoot"><strong>New Twoot </strong>({{ newTwootCharacterCount }}/180)</label>
        <textarea
          id="newTwoot"
          name=""
          rows="4"
          v-model="newTwootContent"
        ></textarea>
        <div class="user-profile_create-twoot-type">
          <label for="newTwootType"><strong>Type</strong></label>
          <select name="" id="newTwootType" v-model="selectedTwootType">
            <option
              :value="option.value"
              v-for="(option, index) in twootTypes"
              :key="index"
            >
              {{ option.name }}
            </option>
          </select>
        </div>
        <button>Post Twoot</button>
      </form>
    </div>
    <div class="user-profile_twoots-wrapper">
      <TwootItem
        v-for="twoot in user.twoots"
        v-bind:key="twoot.id"
        :username="user.username"
        :twoot="twoot"
        @favourite="toggleFavourite"
      />
    </div>
  </div>
</template>

<script>
import TwootItem from "./TwootItem";

export default {
  name: "UserProfile",
  components: { TwootItem },
  data() {
    return {
      newTwootContent: "",
      selectedTwootType: "instant",
      twootTypes: [
        { value: "draft", name: "draft" },
        { value: "instant", name: "Instant Twoot" },
      ],
      followers: 0,
      user: {
        id: 1,
        username: "_slunkeh",
        firstName: "Gavin",
        lastName: "McIntosh",
        email: "slunkeh@gmail.com",
        isAdmin: true,
        twoots: [
          { id: 1, content: "Twotter is amazing!!!" },
          {
            id: 2,
            content: "Dont forget to subscribe to the earth is square...",
          },
        ],
      },
    };
  },
  watch: {
    followers(newFollowerCount, oldFollowerCount) {
      if (oldFollowerCount < newFollowerCount) {
        console.log(`${this.user.username} has gained a new follower`);
      }
    },
  },
  computed: {
    newTwootCharacterCount() {
        return this.newTwootContent.length
    }
  },
  methods: {
    followUser() {
      this.followers++;
    },
    unfollowUser() {
      this.followers--;
    },
    toggleFavourite(id) {
      console.log(`Favourited tweet ${id}`);
    },
    createNewTwoot() {
      if (this.newTwootContent && this.selectedTwootType !== "draft") {
        this.user.twoots.unshift({
          id: this.user.twoots.length + 1,
          content: this.newTwootContent,
        });
        this.newTwootContent = "";
      }
    }
  },
  mounted() {
    this.followUser();
  },
};
</script>

<style lang="scss" scoped>
.user-profile {
  display: grid;
  grid-template-columns: 1fr 3fr;
  padding: 50px 5%;

  .user-profile_user-panel {
    display: flex;
    flex-direction: column;
    margin-right: 50px;
    padding: 20px;
    background-color: white;
    border-radius: 5px;
    border: 1px solid #dfe3e8;

    h1 {
      margin: 0;
    }

    .user-profile_admin-badge {
      background-color: indigo;
      color: white;
      font-weight: 700;
      border-radius: 20px;
      padding: 3px 24px;
      margin-bottom: 9px;
      margin-right: auto;
    }
    .user-profile_create-twoot {
      display: flex;
      flex-direction: column;
      border-top: 1px solid #dfe3e8;
      padding-top: 20px;

      &.--exceeded {
          border: solid 4px red;
          color: red;
      }
    }
  }
}
</style>