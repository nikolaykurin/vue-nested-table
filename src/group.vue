<template>
  <tbody class="minimizable minimized">
    <tr @click="toggleMinimize">
      <th v-for="(column, columnKey) in columns" class="header" :key="`row-${columnKey}`">
          {{ row[column.field] }}
      </th>
      <th class="header actions">
        <a @click.stop.prevent="onEditRow(row.id)" class="action">
          <i class="fa fa-pencil"/>
        </a>
        <a @click.stop.prevent="onDeleteRow(row.id)" class="action">
          <i class="fa fa-remove"/>
        </a>
      </th>
    </tr>
    <tr
      v-if="childrenField"
      is="group-table"
      :parentIndex="index"
      :colspan="colspan"
      :columns="childrenColumns"
      :rows="row[childrenField]"
      @on-row-add="onChildrenAdd"
      @on-row-create="onChildrenCreate"
      @on-row-edit="onChildrenEdit"
      @on-row-selected-action='onChildrenDeleteSelected'/>
  </tbody>
</template>

<script>
  import GroupTable from './group-table';

  export default {
    name: 'group',
    components: {
      GroupTable
    },
    props: {
      index: {
        type: Number
      },
      colspan: {
        type: Number
      },
      columns: {
        type: Array,
        default() {
          // TODO: make from row indexes
          return [];
        }
      },
      row: {
        type: Array,
        default() {
          return [];
        }
      },
      childrenField: {
        type: Number
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
        //
      }
    },
    methods: {
      toggleMinimize(e) {

      },
      onEditRow() {

      },
      onDeleteRow() {

      },
      onChildrenAdd() {

      },
      onChildrenCreate() {

      },
      onChildrenEdit() {

      },
      onChildrenDeleteSelected(data) {
        // data: { action: 'action', ids: [] }
      }
    }
  }
</script>

<style scoped>

</style>