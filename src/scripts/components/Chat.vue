<template v-for="item in $props.chatList">
  <p>{{ item.message }}</p>
</template>

<script>
import VueTypes from 'vue-types';

export default {
  props: {
    chatList: VueTypes.arrayOf(VueTypes.shape({
      message: VueTypes.string.isRequired
    })).isRequired
  }
};
</script>
