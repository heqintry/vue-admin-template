<template>
  <el-dialog title="Assign" :visible.sync="dialogVisible" width="925px">
    <div class="assignDialog-transfer">
      <el-transfer
        v-model="value"
        :data="data"
        @change="handleChange"
        :titles="['Available', 'Assigned']"
      ></el-transfer>
    </div>
  </el-dialog>
</template>

<script>
export default {
  props: {
    visible: {
      type: Boolean,
      required: true
    },
    id: {
      type: String,
      required: true
    },
    available: {
      type: Object,
      required: true
    },
    assigned: {
      type: Object,
      required: true
    }
  },
  watch: {
    visible(value) {
      if (value == true) {
        const data = [];
        const value = [];
        for (let key in this.available) {
          data.push({
            key: key,
            label: "[" + this.available[key] + "] " + key,
            disabled: false
          });
        }
        for (let key in this.assigned) {
          data.push({
            key: key,
            label: "[" + this.assigned[key] + "] " + key,
            disabled: false
          });
          value.push(key);
        }
        this.data = data;
        this.value = value;
      }
    }
  },
  data() {
    return {
      data: [],
      value: []
    };
  },
  computed: {
    dialogVisible: {
      get: function() {
        return this.visible;
      },
      set: function(value) {
        this.$emit("assignShow", value);
      }
    }
  },
  methods: {
    handleChange(value, direction, movedKeys) {
      this.$emit("assignChange", this.id, direction, movedKeys);
    }
  }
};
</script>

<style>
.assignDialog-transfer .el-transfer-panel {
  width: 350px;
  height: 400px;
}
.assignDialog-transfer .el-transfer-panel__body {
  height: 100%;
}
.assignDialog-transfer .el-transfer-panel__list {
  height: 100%;
}
</style>