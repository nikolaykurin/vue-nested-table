<template>
  <div id="vnt">
    <table class="vnt-table">
      <thead>
        <tr>
          <th
            v-for="(column, columnKey) in columns"
            @click="column.sortable ? onSort(column.field) : stub()"
            :key="`column-header-${key}`" :class="`${key === 0 ? 'first' : ''}`">
            <i v-if="columnKey === 0" @click.stop="toggleShowAll" :class="`show-all fa fa-${ showAll ? 'minus' : 'plus' }`" id="show-all"/>
            {{ column.label }}
            <i v-if="column.field === sort.field" :class="`fa fa-long-arrow-${ sort.order === 'asc' ? 'down' : 'up' }`" class="sort" aria-hidden="true"/>
        </tr>
      </thead>
      <tbody
        v-for="(row, rowKey) in rows"
        is="group"
        :index="rowKey"
        :colspan="colspan"
        :row="row"
        :childrenField="childrenField"
        :childrenColumns="childrenColumns"
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
      childrenField: {
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
        showAll: false,
        sort: {
          field: undefined,
          order: undefined
        }
      }
    },
    computed: {
      colspan: (_this) => {
        return _this.columns.length;
      }
    },
    created() {
      //
    },
    mounted() {
      this.$nextTick.then(() => {
        //
      })
    },
    methods: {
      toggleShowAll() {
        // show all or hide all
      },
      toggleMinimize() {

      },
      onSort(field) {
        console.log('sort by: ', field);
        let order = null;
        if (field !== this.sort.field) {
          order = 'asc';
        } else {
          order = this.getSortOrder();
        }

        this.sort.field = field;
        this.sort.order = order;

        this.rows = this.rows.sort((item1, item2) => {
          return this.compareItems(item1, item2, order);
        })
      },
      getSortOrder() {
        switch (this.sort.order) {
          case 'asc':
            return 'desc';
          case 'desc':
            return 'asc';
          default:
            return 'asc';
        }
      },
      compareItems(item1, item2, order) {
        if (item1[field] === item2[field]) {
          return 0;
        }

        if (!item1[field] && item2[field]) {
          return order === 'asc' ? 1 : -1;
        }

        if (!item2[field] && item1[field]) {
          return order === 'asc' ? -1 : 1;
        }

        // TODO: this compares only strings, not numbers
        return order === 'asc' ? (item2[field].localeCompare(item1[field]) > 0 ? 1 : -1) : (item1[field].localeCompare(item2[field]) > 0 ? 1 : -1);
      },
      stub() {
        return false;
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
        tr {
          th {

          }
        }
      }
      tbody {

      }
    }
  }

</style>