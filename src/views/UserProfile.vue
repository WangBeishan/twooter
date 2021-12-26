<template>
  <div class="user-profile">
    <div class="user-profile_user-panel">
      <h1 class="user-profile_username">@{{ state.user.username }}</h1>
      <div class="user-profile_admin-badge" v-if="state.user.isAdmin">
        Admin
      </div>
      <div class="user-profile_follower-count">
        <strong>Followers: </strong> {{ state.followers }}
      </div>
      <CreateTwootPanel
          @add-twoot="addTwoot"
      />
    </div>
    <div class="user-profile_twoots-wrapper">
      <TwootItem
          v-for="twoot in state.user.twoots"
          :key="twoot.id"
          :username="state.user.username"
          :twoot="twoot"
      />
    </div>
  </div>
</template>

<script>
import TwootItem from "../components/TwootItem";
import CreateTwootPanel from "../components/CreateTwootPanel";
import { reactive, computed } from "vue";
import { users } from '../assets/users';
import { useRoute } from 'vue-router'

export default {
  name: "UserProfile",
  components: {
    TwootItem,
    CreateTwootPanel
  },
  setup() {
    const route = useRoute()
    const userId = computed(() => route.params.userId)

    const state = reactive({
      followers: 0,
      user: users[userId.value - 1] || users[0]
    })
    function addTwoot(twootContent) {
      state.user.twoots.unshift({
        id: state.user.twoots.length + 1,
        content: twootContent
      })
    }
    return {
      state,
      addTwoot,
    }
  }
}
</script>

<style lang="scss" scoped>
.user-profile {
  display: grid;
  grid-template-columns: 1fr 3fr;
  grid-gap: 50px;
  padding: 50px 5%;

  .user-profile_user-panel {
    display: flex;
    flex-direction: column;
    padding: 20px;
    background-color: white;
    border-radius: 5px;
    border: 1px solid #DFE3E8;
    margin-bottom: auto;
  }

  h1 {
    margin: 0;
  }

  .user-profile_admin-badge {
    background: rebeccapurple;
    color: white;
    border-radius: 5px;
    margin-right: auto;
    padding: 0 10px;
    font-weight: bold;
  }
  .user-profile_create-twoot {
    padding-top: 20px;
    flex-direction: column;
    display: flex;

    &.exceeded {
      button {
        background-color: red;
      }
    }
  }
}

.user-profile_twoots-wrapper {
  display: grid;
  grid-gap: 10px;
  margin-bottom: auto;
}

</style>
