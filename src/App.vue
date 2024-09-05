<template>
  <div class="small-container">
    <h1>Employees</h1>
    <employee-form @add:employee="addEmployee" />
    <employee-table 
      :employees="employees"   
      @delete:employee="deleteEmployee"
      @edit:employee="editEmployee"
    />
  </div>
</template>

<script>
import EmployeeTable from './components/EmployeeTable.vue';
import EmployeeForm from './components/EmployeeForm.vue';

export default {
  name: 'App',
  components: {
    EmployeeTable,
    EmployeeForm,
  },
  data() {
    return {
      employees: [],
    };
  },
  methods: {
    async addEmployee(employee) { 
      try {
        const response = await fetch('https://jsonplaceholder.typicode.com/users', {
          method: 'POST',
          body: JSON.stringify(employee), 
          headers: { 'Content-Type': 'application/json; charset=UTF-8' },
        });
        const data = await response.json();
        this.employees = [...this.employees, data];
      } catch (error) {
        console.log(error);
      }
    },
    async deleteEmployee(id) {
  // Directly remove employee from local state
  this.employees = this.employees.filter((employee) => employee.id !== id);

  // Optionally, you could still try sending a request to show a network call, but skip actual deletion
  try {
    const response = await fetch('http://jsonplaceholder.typicode.com/users/' + id, {
      method: 'DELETE',
    });

    if (response.ok) {
      console.log('Employee deleted from mock API');
    } else {
      console.error('Failed to delete employee');
    }
  } catch (error) {
    console.log(error);
  }
},


    async editEmployee(id, updatedEmployee) {
      try {
        const response = await fetch(`http://jsonplaceholder.typicode.com/users/${id}`, {
          method: 'PUT',
          body: JSON.stringify(updatedEmployee),
          headers: { 'Content-Type': 'application/json; charset=UTF-8' },
        });
        const data = await response.json();

        this.employees = this.employees.map((employee) =>
          employee.id === id ? data : employee
        );
      } catch (error) {
        console.log(error);
      }
    },
    async getEmployees() {
      try {
        const response = await fetch('https://jsonplaceholder.typicode.com/users', { method: 'GET' });
        const data = await response.json();
        this.employees = data;
      } catch (error) {
        console.log(error);
      }
    },
  },
  mounted() {
    this.getEmployees();
  },
};
</script>

<style>
.button {
  background-color: #009435;
  border: 1px solid #009435; 
}

.small-container {
  max-width: 680px;
}
</style>
