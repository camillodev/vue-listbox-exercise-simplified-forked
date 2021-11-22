<template>
  <div class="container py-3">
    <h3>Formula 1 Top Drivers</h3>
    <div class="py-3 px-3">
      <div class="row">
        <div class="col">
          <label for="filters" class="form-label">Filter</label>
          <select class="form-select" id="filters" @change="selectFilter">
            <!-- TODO: render filter options -->
            <option
              v-for="filter in filters"
              :key="filter.id"
              :value="filter.name"
            >
              {{ filter.name }}
            </option>
          </select>
        </div>
      </div>
      <div class="mb-3"></div>
      <div class="row">
        <div class="col">
          <div class="border border-1">
            <div class="row px-2 py-2">
              <div class="col">
                <div class="form-check">
                  <input
                    class="form-check-input"
                    type="checkbox"
                    id="selectAll"
                    :checked="allItemsSelected"
                    @change="selectAll"
                  />
                  <label class="form-check-label" for="selectAll">
                    Select All
                  </label>
                </div>
              </div>
              <!-- TODO: render correct totals (i.e. 1 of 5 selected)-->
              <div class="col text-end">
                {{ selectedItems.length }} of {{ items.length }} selected
              </div>
            </div>
            <div class="row px-2 py-2">
              <div class="col">
                <!-- TODO: render collection of checkboxes -->
                <div v-for="item in activeItems" class="my-1" :key="item.id">
                  <input
                    class="form-check-input"
                    type="checkbox"
                    :checked="isItemChecked(item)"
                    :id="item.id"
                    @change="(e) => selectItem(e, item.id)"
                  />
                  <label class="form-check-label mx-2" for="1">
                    {{ item.name }}
                  </label>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
const FILTER_OPTIONS = Object.freeze({
  ALL: {
    id: 1,
    name: "All",
  },
  SELECTED: {
    id: 2,
    name: "Selected",
  },
  UNSELECTED: {
    id: 3,
    name: "Unselected",
  },
});

export default {
  name: "ListBox",

  props: {
    items: {
      type: Array,
      default: () => [],
    },
  },

  data() {
    return {
      filters: Object.values(FILTER_OPTIONS),
      selectedFilter: null,
      selectedItems: [],
    };
  },

  computed: {
    activeItems() {
      // TODO: return the collection of items depending on their "status" (i.e. checked, unchecked or all)
      if (this.selectedFilter === FILTER_OPTIONS.ALL) {
        return this.items;
      } else if (this.selectedFilter === FILTER_OPTIONS.SELECTED) {
        return this.items.filter((item) =>
          this.selectedItems.includes(item.id)
        );
      } else if (this.selectedFilter === FILTER_OPTIONS.UNSELECTED) {
        return this.items.filter(
          (item) => !this.selectedItems.includes(item.id)
        );
      }
      return this.items;
    },

    allItemsSelected() {
      // return boolean depending if all items are selected
      return this.items.find((item) => !this.selectedItems.includes(item.id))
        ? false
        : true;
    },
  },

  methods: {
    isItemChecked(item) {
      // TODO: return boolean depending if the item is already in our selectedItems list
      return this.selectedItems.includes(item.id);
    },

    selectFilter(e) {
      // TODO: set this.selectedFilter depending the e.target.value
      if (e.target.value === "All") {
        this.selectedFilter = FILTER_OPTIONS.ALL;
      } else if (e.target.value === "Selected") {
        this.selectedFilter = FILTER_OPTIONS.SELECTED;
      } else if (e.target.value === "Unselected") {
        this.selectedFilter = FILTER_OPTIONS.UNSELECTED;
      }
    },

    selectAll(e) {
      // TODO: select all or deselect all items depending on e.target.checked
      // TODO: emit selectedItems to parent component
      if (e.target.checked) {
        this.selectedItems = this.items.map((item) => item.id);
      } else {
        this.selectedItems = [];
      }

      this.$emit("selected-items", this.selectedItems);
    },

    selectItem(e, itemId) {
      // TODO: add or remove an item depending on e.target.checked
      // TODO: emit selectedItems to parent component
      if (e.target.checked) {
        const item = this.items.find((item) => item.id === itemId);
        if (!this.isItemChecked(item)) {
          this.selectedItems.push(itemId);
        }
      } else {
        const itemIndex = this.selectedItems.findIndex(
          (item) => item === itemId
        );
        if (itemIndex >= 0) {
          this.selectedItems.splice(itemIndex, 1);
        }
      }
      this.$emit("selected-items", this.selectedItems);
    },
  },
};
</script>
