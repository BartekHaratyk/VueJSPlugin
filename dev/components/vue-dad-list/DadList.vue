<template>
  <div>
    <vddl-list
      :list="list"
      :allowed-types="allowedTypes"
      :horizontal="false"
      class="panel__body--list">
      <template v-for="(item, index) in list">
        <vddl-draggable
          :type="item.type"
          :key="item.id"
          :draggable="item"
          :index="index"
          :wrapper="list"
          class="panel__body--item"
          effect-allowed="move">
          <slot
            :item="item"
            :index="index"
            :lastOpendItemId="lastOpendItemId"/>
        </vddl-draggable>
      </template>
      <slot name="placeholder">
        <vddl-placeholder>
          <div
            :is="itemType"
            :item="dummyItem"
            :index="99999"/>
        </vddl-placeholder>
      </slot>
    </vddl-list>
    <slot
      :add="addItemEvent"
      name="addArea">
      <vue-button
        title="Add"
        @click.native="addItemEvent"/>
    </slot>
  </div>
</template>

<script>
export default {
  name: 'DadList',
  props: {
    'list':{
      type: [Array,Object],
      required: true
    },
    'itemType': {
      type: String,
      default: ''
    },
    'allowedTypes': {
      type: Array,
      default: null
    }
  },
  data: function() {
    return {
      dummyItem: {
        "id": 5,
        "label": "",
        "collapsed": true
      },
      lastOpendItemId: null,
      DaDList: true
    };
  },
  created: function() {
    this.$on('lastOpend', this.setLastOpendId);
  },
  methods: {
    addItemEvent: function() {
      this.$emit("addItem");
    },
    setLastOpendId: function(newId) {
      this.lastOpendItemId = newId;
    }
  }
};
</script>
<style lang="scss">
.vddl-list {
    min-height: 24px;
}
.vddl-list, .vddl-draggable {
    position: relative;
}
.vddl-dragging {
    opacity: .7;
}
.vddl-placeholder{
    opacity: 0.4;
}

.vddl-dragging-source {
    display: none;
}
</style>
