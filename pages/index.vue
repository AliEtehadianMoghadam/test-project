<template>
<v-container>
  <Form />
  <Table/>
</v-container>

</template>

<script>
import Form from '../components/form.vue'
import Table from '../components/Table.vue'
export default {
  name: 'IndexPage',
  components: { Form, Table },
}
</script>
