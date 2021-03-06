<template>
  <div>
    <b-row class="mb-2">
      <b-col class="d-flex">
        <input class="form-control mr-2 flex-grow-1" v-model="filter" :placeholder="$t('Filter')">
        <b-btn class="mr-2" size="sm" variant="primary" @click.stop="search" style="width:6em;">
          <i class="fas fa-search" />
        </b-btn>
        <b-btn class="text-nowrap" size="sm" variant="secondary" @click.stop="displayFormProperty">
          <i class="fas fa-plus" />
          {{ $t('Watcher') }}
        </b-btn>
      </b-col>
    </b-row>

    <b-table :items="filtered" :fields="fields" responsive striped bordered small hover fixed>
      <template slot="HEAD_property" slot-scope="data">{{ $t(data.label) }}</template>
      <template slot="HEAD_actions" slot-scope="data">{{ $t(data.label) }}</template>

      <template v-slot:cell(actions)="row">
        <!-- we use @click.stop here to prevent emitting of a 'row-clicked' event  -->
        <a
          size="lg"
          variant="action"
          :title="$t('edit')"
          class="btn btn-lg p-0 mr-2 border-0 bg-transparent"
          @click.stop="editProperty(row.item)"
        >
          <i class="fa fa-edit fa-1x" />
        </a>
        <a
          size="lg"
          variant="action"
          :title="$t('Delete')"
          class="btn btn-lg p-0 mr-2 border-0 bg-transparent"
          @click.stop="deleteProperty(row.item)"
        >
          <i class="fa fa-trash fa-1x" />
        </a>
      </template>
      <!-- Keeps compatibility with the bootstrap-vue version in ProcessMaker -->
      <template slot="actions" slot-scope="row">
        <!-- we use @click.stop here to prevent emitting of a 'row-clicked' event  -->
        <a
          size="lg"
          variant="action"
          :title="$t('edit')"
          class="btn btn-lg p-0 mr-2 border-0 bg-transparent"
          @click.stop="editProperty(row.item)"
        >
          <i class="fa fa-edit fa-1x" />
        </a>
        <a
          size="lg"
          variant="action"
          :title="$t('Delete')"
          class="btn btn-lg p-0 mr-2 border-0 bg-transparent"
          @click.stop="deleteProperty(row.item)"
        >
          <i class="fa fa-trash fa-1x" />
        </a>
      </template>
    </b-table>
    <template slot="modal-footer">
      <span />
    </template>
  </div>
</template>

<script>
import { FormInput, FormTextArea } from '@processmaker/vue-form-elements';

export default {
  components: {
    FormInput,
    FormTextArea,
  },
  props: {
    value: {
      type: Array,
      default: () => [],
    },
  },
  data() {
    return {
      filter: '',
      fields: [
        {
          key: 'name',
          label: this.$t('Name'),
          class: 'text-center',
          sortable: true,
        },
        {
          key: 'watching',
          label: this.$t('Watching'),
          class: 'text-center',
          sortable: true,
        },
        {
          key: 'output_variable',
          label: this.$t('Variable'),
          class: 'text-center',
          sortable: true,
        },
        {
          key: 'script.title',
          label: this.$t('Script'),
          class: 'text-center',
          sortable: true,
        },
        {
          key: 'actions',
          label: '',
          class: 'text-center',
          sortable: false,
        },
      ],
    };
  },
  computed: {
    filtered() {
      if (!this.filter) {
        return this.value;
      }
      const filtered = [];
      this.value.forEach(item => {
        if (Object.keys(item).find(key => typeof item[key] === 'string' ? item[key].indexOf(this.filter)>=0 : false)) {
          filtered.push(item);
        }
      });
      return filtered;
    },
  },
  methods: {
    search() {

    },
    displayFormProperty() {
      this.$emit('display-form');
    },
    editProperty(item) {
      this.$emit('edit-form', item);
    },
    deleteProperty(item) {
      this.$emit('delete-form', item);
    },
  },
};
</script>
