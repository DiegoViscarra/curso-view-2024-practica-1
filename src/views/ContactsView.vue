<template>
  <div>
    <h1>{{ title }}</h1>
    <ContactFilterComponent @filter-contacts="applyFilter" />
    <table>
      <thead>
        <tr>
          <th>ID</th>
          <th>Name</th>
          <th>Email</th>
          <th>Address</th>
          <th>Phone</th>
          <th>Country</th>
          <th>City</th>
          <th></th>
        </tr>
        <tr>
          <th><button @click="addContact()">Add</button></th>
          <th><input type="text" v-model="newContact.name"></th>
          <th><input type="text" v-model="newContact.email"></th>
          <th><input type="text" v-model="newContact.address"></th>
          <th><input type="text" v-model="newContact.phone"></th>
          <th><input type="text" v-model="newContact.country"></th>
          <th><input type="text" v-model="newContact.city"></th>
        </tr>
        <tr v-if="editIndex !== null">
          <th><button @click="saveContact()">Save</button></th>
          <th><input type="text" v-model="currentContact.name"></th>
          <th><input type="text" v-model="currentContact.email"></th>
          <th><input type="text" v-model="currentContact.address"></th>
          <th><input type="text" v-model="currentContact.phone"></th>
          <th><input type="text" v-model="currentContact.country"></th>
          <th><input type="text" v-model="currentContact.city"></th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(contact, index) in filteredContacts" :key="contact.id">
          <td>{{contact.id}}</td>
          <td>{{contact.name}}</td>
          <td>{{contact.email}}</td>
          <td>{{contact.address}}</td>
          <td>{{contact.phone}}</td>
          <td>{{contact.country}}</td>
          <td>{{contact.city}}</td>
          <td>
            <button @click="deleteContact(index)">Delete</button>
            <button @click="editContact(contact, index)">Edit</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import ContactFilterComponent from '../components/ContactFilterComponent.vue';

export default {
  name: 'ContactsView',
  components: {
    ContactFilterComponent
  },
  data() {
    return {
      title: 'Contact List',
      newContact: { 
        id: null, 
        name: '', 
        email: '', 
        address: '', 
        phone: '',
        country: '',
        city: ''
      },
      currentContact: { 
        id: null, 
        name: '', 
        email: '', 
        address: '', 
        phone: '',
        country: '',
        city: ''
      },  
      editIndex: null, 
      contacts: [
        { id: 1, name: 'Alice Johnson', email: 'alice.johnson@example.com', address: '123 Maple Street', phone: '123-456-7890', country: 'USA', city: 'New York' },
        { id: 2, name: 'Bob Smith', email: 'bob.smith@example.com', address: '456 Oak Avenue', phone: '987-654-3210', country: 'Canada', city: 'Toronto' },
        { id: 3, name: 'Carol White', email: 'carol.white@example.com', address: '789 Pine Road', phone: '555-123-4567', country: 'UK', city: 'London' },
        { id: 4, name: 'David Brown', email: 'david.brown@example.com', address: '321 Elm Street', phone: '444-555-6666', country: 'Australia', city: 'Sydney' },
        { id: 5, name: 'Emily Davis', email: 'emily.davis@example.com', address: '654 Spruce Lane', phone: '333-444-5555', country: 'USA', city: 'Los Angeles' }
      ],
      filterQuery: ''
    }
  },
  computed: {
    filteredContacts() {
      const query = this.filterQuery.toLowerCase();
      return this.contacts.filter(contact => 
        contact.name.toLowerCase().includes(query) ||
        contact.email.toLowerCase().includes(query)
      );
    }
  },
  methods: {
    addContact(){
      const lastId = this.contacts.length ? Math.max(...this.contacts.map(c => c.id)) : 0;
      this.newContact.id = lastId + 1;
      this.contacts.push(Object.assign({}, this.newContact));
    },
    saveContact(){
      this.contacts[this.editIndex] = Object.assign({}, this.currentContact);
      this.editIndex = null;
    },
    editContact(contact, index){
      this.editIndex = index;
      this.currentContact = Object.assign({}, contact);
    },
    deleteContact(index){
      this.contacts.splice(index, 1);
    },
    applyFilter(query) {
      this.filterQuery = query;
    }
  }
}
</script >

<style scope>
h1 {
  color: #42b983;
}
table {
  width: 100%;
  border-collapse: collapse;
}
th, td {
  border: 1px solid #ddd;
  padding: 8px;
}
th {
  background-color: #f2f2f2;
  text-align: left;
}
</style>