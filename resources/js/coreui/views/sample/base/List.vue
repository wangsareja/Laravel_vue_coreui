<template>
  <b-card>
    <template slot="header">
      <slot name="caption">Table</slot>
    </template>
    <b-table
      :hover="hover"
      :striped="striped"
      :bordered="bordered"
      :small="small"
      :fixed="fixed"
      :items="items"
      :fields="fields"
      :current-page="currentPage"
      :per-page="perPage"
      responsive="sm"
    >
      <template slot="status" slot-scope="data">
        <b-badge :variant="getBadge(data.item.status)">{{ data.item.status }}</b-badge>
      </template>
    </b-table>
    <nav>
      <b-pagination
        v-model="currentPage"
        :total-rows="getRowCount(items)"
        :per-page="perPage"
        prev-text="Prev"
        next-text="Next"
        hide-goto-end-buttons
      />
    </nav>
  </b-card>
</template>

<script>
import { constants } from "crypto";

export default {
  name: "CTable",
  props: {
    hover: {
      type: Boolean,
      default: false
    },
    striped: {
      type: Boolean,
      default: false
    },
    bordered: {
      type: Boolean,
      default: false
    },
    small: {
      type: Boolean,
      default: false
    },
    fixed: {
      type: Boolean,
      default: false
    }
  },
  data: () => {
    return {
      items: [],
      itemsModif: [],
      fields: [
        { key: "id" },
        { key: "employee_name" },
        { key: "employee_salary" },
        { key: "employee_age" },
        { key: "profile_image" }
      ],
      currentPage: 1,
      perPage: 5,
      totalRows: 0,
    };
  },
  mounted() {
    axios
      .get("http://dummy.restapiexample.com/api/v1/employees")
      .then(response => (this.items = response.data))
      .catch(error => console.log(error));
  },
  computed: {
    add: function() {}
  },
  methods: {
    getBadge(status) {
      return status === "Active"
        ? "success"
        : status === "Inactive"
        ? "secondary"
        : status === "Pending"
        ? "warning"
        : status === "Banned"
        ? "danger"
        : "primary";
    },
    getRowCount(items) {
      return items.length;
    }
  }
};
</script>
