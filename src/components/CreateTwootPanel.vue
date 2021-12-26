<template>
  <form class="user-profile_create-twoot" @submit.prevent="createNewTwoot" :class="{'exceeded': newTwootCharacterCount > 180}">
    <label for="newTwoot"><strong>New Twoot</strong> ({{ newTwootCharacterCount }}/180)</label>
    <textarea id="newTwoot" rows="4" v-model="state.newTwootContent"></textarea>
    <div class="user-profile_create-twoot-type">
      <label for="newTwootType"><strong>Type: </strong></label>
      <select id="newTwootType" v-model="state.selectTwootType">
        <option v-for="(option, index) in state.twootTypes" :key="index" :value="option.value">
          {{ option.name }}
        </option>
      </select>
    </div>
    <button type="submit">
      Twoot!
    </button>
  </form>
</template>

<script>
import { reactive, computed } from "vue";

export default {
  name: "CreateTwootPanel",
  setup(props, ctx) {
    const state = reactive({
      newTwootContent: "",
      selectTwootType: "instant",
      followers: 0,
      twootTypes: [
        { value: 'draft', name: 'Draft'},
        { value: 'instant', name: 'Instant Twoot'}
      ],
    })

    const newTwootCharacterCount = computed(() => state.newTwootContent.length)

    function createNewTwoot() {
      if (state.newTwootContent && state.selectTwootType != 'draft') {
        ctx.emit('add-twoot', state.newTwootContent)
        state.newTwootContent = ''
      }
    }

    return {
      state,
      newTwootCharacterCount,
      createNewTwoot
    }
  }
}
</script>

<style scoped>

</style>
