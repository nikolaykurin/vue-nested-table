<template>
  <div id="vnt">
    <table class="vnt-table">
      <thead>
        <tr>
          <th v-for="(column, key) in columns" :key="`header-${key}`">{{ column.label }}</th>
        </tr>
      </thead>
      <tbody
        v-for="(row, rowKey) in rows"
        is="group"
        :colspan="colspan"
        :row="row"
        :children="children"
        @toggle="toggleMinimize"
        @on-row-edit="onRowEdit"
        @on-row-delete="onRowDelete"
        @on-children-add="onChildrenAdd"
        @on-children-create="onChildrenCreate"
        @on-children-edit="onChildrenEdit"
        @on-selected-children-action="onSelectedChildrenAction"
        :key="`tbody-${rowKey}`"/>
    </table>
  </div>
</template>

<script>
  import Group from './group';

  export default {
    name: 'table',
    components: {
     Group
    },
    props: {
      columns: {
        type: Array,
        default() {
          return [];
        }
      },
      rows: {
        type: Array,
        default() {
          return [];
        }
      },
      children: {
        type: String,
        default() {
          return null;
        }
      },
      childrenColumns: {
        type: Array,
        default() {
          return [];
        }
      }
    },
    data() {
      return {
        showAll: false
      }
    },
    computed: {
      colspan: (_this) => {
        return _this.columns.length;
      }
    },
    created() {

    },
    methods: {
      toggleShowAll() {
        // show all or hide all
      },
      toggleMinimize() {

      },
      onRowCreate() {

      },
      onRowEdit() {

      },
      onRowDelete() {

      },
      onChildrenAdd() {

      },
      onChildrenCreate() {

      },
      onChildrenEdit() {

      },
      onSelectedChildrenAction() {

      }
    }
  }
</script>

<style scoped>

</style>

<style lang="scss">

  #vnt {
    .vnt-table {
      thead {

      }
      tbody {

      }
    }
  }

</style>