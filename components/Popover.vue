<template>
  <transition name="fade">
    <aside
      class="popover-wrapper"
      v-if="popover">
      <div class="backshadow" @click="closePopover()" />
      <div class="popover">
        <span @click="closePopover()" class="button-close clickable">&times;</span>
        <div
          class="content"
          v-if="content && content !== 'reference'"
          v-html="content" />
        <slot v-else-if="content && content === 'reference'" />
        <h2 v-else>Nothing found</h2>
      </div>
    </aside>
  </transition>
</template>

<script>
  import { mapState, mapActions } from 'vuex'

  export default {
    computed: {
      ...mapState({
        'popover': state => state.popover.popover,
        'content': state => state.popover.content
      })
    },
    methods: {
      ...mapActions([
        'closePopover'
      ])
    }
  }
</script>

<style lang="scss">
  @import "~@/assets/style/global";

  .fade-enter-active, .fade-leave-active {
    transition: opacity .5s;
  }
  .fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
    opacity: 0;
  }

  .popover-wrapper, .backshadow {
    position: fixed;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    width: 100vw;
    height: 100vh;
  }

  .popover-wrapper {
    display: flex;
    align-items: center;
    justify-content: center;
    z-index: 100;

    .backshadow {
      background-color: rgba(0, 0, 0, 0.4);
      z-index: 1;
    }

    .popover {
      width: 50vw;
      height: 80vh;
      background-color: rgba(255, 255, 255, 1);
      box-shadow: 0 2px 10px rgba(0, 0, 0, .2);
      padding: $spacing * 1.5;
      overflow-y: scroll;
      z-index: 2;
      position: relative;

      .button-close {
        position: absolute;
        right: $spacing * 1.2;
        top: $spacing;
        font-size: $size-big;
        line-height: 1;
      }

      & > * {
        font-size: $size-small;
        max-width: 700px;
      }

      .content {
        h3 {
          margin-top: $spacing;
          text-transform: capitalize;
        }

        ul {
          margin: $spacing / 2 0 $spacing / 2 $spacing / 3;
        }
      }
    }
  }
</style>
