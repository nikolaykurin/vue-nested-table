<template>
  <div class="table-block">
    <table v-if="rows.length" class="table">
      <thead>
        <tr>
          <th><!-- checkbox-column --></th>
          <th v-for="(column, columnKey) in columns" :key="`editable-table-header-${columnKey}`">{{ column.label }}</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(row, rowKey) in rows" :key="`editable-table-tbody-${rowKey}`">
          <th>
            <input
              @change="toggleChecked(row.id)"
              type="checkbox"
              :checked="checked_list.includes(row.id)"/>
          </th>
          <th v-for="(column, columnKey) in columns" :key="`editable-table-row-${columnKey}`">
            <template v-if="column.editable">
              <template v-if="['text', 'number'].includes(column.type)">
                <input
                  v-model="row[column.field]"
                  :type="column.type"
                  class="cell editable"
                  @blur="onCellBlur($event, rowKey)"/>
              </template>
              <template v-else-if="column.type === 'select'">
                <!-- TODO: add dictionaries -->
              </template>
              <template v-else-if="column.type === 'radio'">
                <div class="cell cell-div">
                  <input
                    type="radio"
                    id="radio-yes"
                    value="true"
                    v-model="row[column.field]"
                    :checked="row[column.field] === 1"
                    @change="onCellBlur($event, rowKey)">
                  <label for="radio-yes">YES</label>
                  <input
                    type="radio"
                    id="radio-no"
                    value="false"
                    v-model="row[column.field]"
                    :checked="row[column.field] === 0"
                    @change="onCellBlur($event, rowKey)">
                  <label for="radio-no">NO</label>
                </div>
              </template>
            </template>
            <template v-else>
              <span class="cell">{{ row[column.field] }}</span>
            </template>
          </th>
        </tr>
      </tbody>
    </table>
    <template v-else>
      {{ no_data_message}}
    </template>
    <div class="actions">
      <a @click="onAddRow" class="action" tabindex="1">
        <i class="fa fa-plus"/>
        Add
      </a>
      <a v-if="checked_list.length" @click="onDeleteChecked" class="action" tabindex="2">
        <i class="fa fa-close"/>
        Delete
      </a>
    </div>
  </div>
</template>

<script>
  import { confirmation } from './utils';

  export default {
    name: 'group-table',
    props: {
      parentIndex: {
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
      rows: {
        type: Array
      }
    },
    data() {
      return {
        no_data_message: 'No Data',
        checked_list: []
      }
    },
    methods: {
      onCellBlur(e, row_index) {
        const row = this.rows[row_index];

        if (!this.validateRow(row)) {
          return;
        }

        if (row.id) {
          this.$emit('on-row-update', row);
        } else {
          this.$emit('on-row-create', { row: row, index: row_index });
        }
      },
      onAddRow() {
        this.$emit('on-row-add');
      },
      async onDeleteChecked() {
        if (!confirmation()) {
          return;
        }

        this.$emit('on-row-selected-action', { action: 'delete', ids: this.checked_list });

        this.checked_list = [];
      },
      toggleChecked(id) {
        if (this.checked_list.includes(id)) {
          this.checked_list.splice(this.checked_list.indexOf(id), 1);
        } else {
          this.checked_list.push(id);
        }
      },
      validateRow(row) {
        let validated = true;

        this.columns.forEach((column) => {
          if (column.required && !row[column.field]) {
            validated = false;
          }
        });

        return validated;
      }
    }
  }
</script>

<style lang="scss" scoped>

  .table-block {
    .table {

    }
    .actions {

    }
  }

</style>