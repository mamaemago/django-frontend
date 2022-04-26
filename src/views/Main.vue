<template>
  <div class="container">
    <div class="structure">
      <div>
        <h1>Students Database</h1>
        <br />
      </div>
      <div>
        <form>
          <input v-model="formData.name" placeholder="Student Name" />
          <input v-model="formData.course" placeholder="Course" />
          <input v-model="formData.rating" placeholder="Rating" />
          <button
            v-if="!editMode"
            @click.prevent="addStudent"
            :disabled="!formData.name || !formData.course || !formData.rating"
          >
            Add
          </button>
          <button
            v-if="!editMode"
            @click.prevent="reset"
            class="resetButton"
            :disabled="!formData.name && !formData.course && !formData.rating"
          >
            Reset
          </button>
          <button v-if="editMode" @click.prevent="saveModifiedStudent">
            Save
          </button>
          <button class="resetButton" v-if="editMode" @click.prevent="cancel">
            Cancel
          </button>
        </form>
      </div>
      <div>
        <Table
          :dataset="dataset"
          @tableEditClick="editStudent"
          @tableRemoveClick="deleteStudent"
        />
      </div>
    </div>
  </div>
</template>

<script>
import { ref } from '@vue/reactivity';
import Table from '../components/Table.vue';
import Services from '../services/services';

export default {
  name: 'MainPage',
  components: { Table },
  setup() {
    const dataset = ref([]);

    async function loadDataBase() {
      try {
        let req = await Services.getStudents();
        if (req) {
          dataset.value = req;
          cancel();
        }
      } catch (error) {
        console.log(error);
      }
    }

    loadDataBase();

    const formData = ref({ id: '', name: '', course: '', rating: '' });

    async function addStudent() {
      try {
        let req = await Services.addStudent(
          formData.value.name,
          formData.value.course,
          formData.value.rating,
        );
        if (req) {
          await loadDataBase();
          editMode.value = false;
        }
      } catch (error) {
        console.log(error);
      }
    }

    const editMode = ref(false);

    function reset() {
      formData.value.name = '';
      formData.value.course = '';
      formData.value.rating = '';
      formData.value.id = '';
    }

    function cancel() {
      reset();
      editMode.value = false;
    }

    function editStudent(student) {
      editMode.value = true;
      formData.value.name = student.name;
      formData.value.course = student.course;
      formData.value.rating = student.rating;
      formData.value.id = student.id;
    }

    async function saveModifiedStudent() {
      try {
        let req = await Services.editStudent(
          formData.value.id,
          formData.value.name,
          formData.value.course,
          formData.value.rating,
        );
        if (req) await loadDataBase();
      } catch (e) {
        console.log(e);
      }
    }

    async function deleteStudent(student) {
      try {
        await Services.deleteStudent(student.id);
      } catch (e) {
        console.log(e);
      }
      await loadDataBase();
    }

    return {
      dataset,
      formData,
      reset,
      addStudent,
      editMode,
      editStudent,
      saveModifiedStudent,
      deleteStudent,
      cancel,
    };
  },
};
</script>
