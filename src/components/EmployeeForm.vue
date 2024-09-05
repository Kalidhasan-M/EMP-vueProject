<template>
  <div id="employee-form">
    <form @submit.prevent="handleSubmit"> 
      <label>Employee Name</label>
      <input 
        type="text" 
        v-model="employee.name"
        :class="{'has-error': submitting && invalidName}" 
        @focus="clearStatus"
        @keypress="clearStatus"
         ref="firstinput"

      />
      <label>Employee Email</label>
      <input 
        type="text" 
        v-model="employee.email" 
        :class="{'has-error': submitting && invalidEmail}"
        @focus="clearStatus"
        @keypress="clearStatus"
      />
      <p v-if="submitting && error" class="error-message">please fill out all the required fields!</p>
      <p v-if="success" class="success-message">Employee successfully added!</p>
      <button type="submit">Add Employee</button>
    </form>
  </div>
</template>

<script>
export default {
  name: 'employee-form',
  data() {
    return {
      submitting: false,
      success: false,
      error: false,
      employee: {
        name: '',
        email: ''
      }
    };
  },
  methods: {
    handleSubmit() {
      this.submitting = true;  // Corrected typo here
      this.clearStatus();

      if (this.invalidName || this.invalidEmail) {
        this.error = true; 
        return;
      }

      this.$emit('add:employee', this.employee);
this.$refs.firstinput.focus();

      this.employee = {
        name: '',
        email: ''
      };
      this.success = true; 
      this.error = false;
      this.submitting = false;
    },
    clearStatus() {
      this.success = false;
      this.error = false;
    }
  },
  computed: {
    invalidName() {
      return this.employee.name === '';
    },
    invalidEmail() {
      return this.employee.email === '';
    }
  }
};
</script>

<style scoped>
form {
  margin-bottom: 2rem;
}

input.has-error,
input.has-error:hover,
input.has-error:focus,
input.has-error:active {
  border: 1px solid #d33c40;
  box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.1), 0 0 6px #f4cecf;
}
[class*='-message'] {
  font-weight: 500;

}
.error-message{
  color: #d33c40;
}
.success-message {
  color: #32a95d;
}

</style>
