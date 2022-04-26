<template>
  <section v-if="dataset && dataset.length > 0">
    <!--for demo wrap-->
    <div class="tbl-header">
      <table cellpadding="0" cellspacing="0" border="0">
        <thead>
          <tr>
            <th>Name</th>
            <th>Course</th>
            <th>Rating</th>
            <th>Actions</th>
          </tr>
        </thead>
      </table>
    </div>
    <div class="tbl-content">
      <table cellpadding="0" cellspacing="0" border="0">
        <tbody>
          <tr v-for="student in dataset" :key="student.id">
            <td>{{ student.name }}</td>
            <td>{{ student.course }}</td>
            <td>{{ student.rating }}</td>
            <td>
              <button class="editButton" @click="tableEditClick(student)">
                Edit
              </button>
              <button class="deleteButton" @click="tableRemoveClick(student)">
                Remove
              </button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </section>
  <div v-else>
    <br />
    <h2>Ops, no data found :(</h2>
  </div>
</template>

<script>
export default {
  name: 'TableComponent',
  props: { dataset: Array },
  emits: ['tableEditClick', 'tableRemoveClick'],
  setup(props, { emit }) {
    function tableEditClick(student) {
      emit('tableEditClick', student);
    }
    function tableRemoveClick(student) {
      emit('tableRemoveClick', student);
    }
    return { tableEditClick, tableRemoveClick };
  },
};
</script>

<style scoped>
h1 {
  font-size: 30px;
  color: #fff;
  text-transform: uppercase;
  font-weight: 300;
  text-align: center;
  margin-bottom: 15px;
}
table {
  width: 100%;
  /* table-layout: fixed; */
}
.tbl-header {
  background-color: rgba(255, 255, 255, 0.3);
}
.tbl-content {
  margin-top: 0px;
  border: 1px solid rgba(255, 255, 255, 0.3);
}

th {
  padding: 20px 15px;
  font-weight: 500;
  font-size: 14px;
  color: #fff;
  text-transform: uppercase;
  width: 25%;
}
td {
  padding: 15px;
  width: 25%;
  vertical-align: middle;
  font-weight: 300;
  font-size: 12px;
  color: #fff;
  border-bottom: solid 1px rgba(255, 255, 255, 0.1);
}

/* demo styles */

section {
  margin: 50px;
}

.editButton:hover {
  background: #17adf4;
  /* border: solid 1px #17adf4; */
}

.deleteButton:hover {
  background: #f8415d;
  /* border: solid 1px #f8415d; */
}
</style>
