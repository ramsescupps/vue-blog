<template>
  <div class="tag-wall">
    <panel :title="$t('common.tagsWallTitle')">
      <div slot="content" class="content">
        <i-tag :color="tag.color" type="dot" v-for="tag in tags" :key="tag.id" style="margin: 0 5px 5px 0;" class="dot-tag">
          {{tag[resolveI18N('name')]}} [{{ tag.related_post_num }}]
        </i-tag>
      </div>
    </panel>
  </div>
</template>

<script type="text/ecmascript-6">
  import {
    mapState,
    mapActions
  } from 'vuex';
  import Panel from '@/components/views/Panel';
  import {mixin} from '@/common/js/utils';

  export default {
    name: 'tag-wall',
    mixins: [mixin],
    asyncData({store, route}) {
      return Promise.all([
        store.dispatch('common/GET_TAGS')
      ]);
    },
    mounted() {
      if (this.$store.state.common.tags.length === 0) {
        this['common/GET_TAGS']();
      }
    },
    computed: {
      ...mapState({
        tags: state => state.common.tags
      })
    },
    methods: {
      ...mapActions(['common/GET_TAGS'])
    },
    components: {
      'panel': Panel
    }
  };
</script>

<style lang="stylus" type="text/stylus" rel="stylesheet/stylus">

  .tag-wall
    .content
      padding 5px 20px
      border-left 1px solid $default-border-color

</style>
