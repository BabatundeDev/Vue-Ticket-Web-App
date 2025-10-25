<template>
  <div class="dashboard-container">
    <div class="dashboard-header">
      <h2>Welcome to Your Dashboard</h2>
      <p>This is a protected page only visible to logged-in users.</p>
    </div>

    <div class="filters">
      <select v-model="filterStatus">
        <option value="All">All Statuses</option>
        <option value="Open">Open</option>
        <option value="In Progress">In Progress</option>
        <option value="Closed">Closed</option>
      </select>
      <input
        type="text"
        placeholder="Filter by assignee"
        v-model="filterAssignee"
      />
    </div>

    <div class="ticket-list">
      <h3>Your Tickets</h3>
      <div style="display: flex; flex-wrap: wrap; gap: 1rem; margin-top: 1rem;">
        <div
          v-for="ticket in filteredTickets"
          :key="ticket.id"
          class="ticket-card"
        >
          <h4>{{ ticket.title }}</h4>
          <p>
            <strong>Status:</strong>
            <span :class="['ticket-status', ticket.status.toLowerCase().replace(' ', '-')]">
              {{ ticket.status }}
            </span>
          </p>
          <p><strong>Assigned to:</strong> {{ ticket.assignedTo }}</p>

          <div class="ticket-actions">
            <button class="edit" @click="editTicket(ticket)">Edit</button>
            <button class="delete" @click="deleteTicket(ticket.id)">Delete</button>
          </div>
        </div>
      </div>
    </div>

    <div class="ticket-form">
      <h3>{{ editingTicket ? 'Edit Ticket' : 'Create New Ticket' }}</h3>
      <form @submit.prevent="handleSubmit">
        <input v-model="form.title" required placeholder="Title" />
        <input v-model="form.assignedTo" required placeholder="Assigned To" />
        <select v-model="form.status" required>
          <option value="Open">Open</option>
          <option value="In Progress">In Progress</option>
          <option value="Closed">Closed</option>
        </select>
        <button
          type="submit"
          :class="editingTicket ? 'update' : 'create'"
          class="ticket-form-button"
        >
          {{ editingTicket ? 'Update Ticket' : 'Create Ticket' }}
        </button>
      </form>
    </div>

    <button class="logout-button" @click="logout">Logout</button>
  </div>
</template>

<script>
import { toast } from 'vue3-toastify'

export default {
  data() {
    return {
      tickets: [],
      filterStatus: 'All',
      filterAssignee: '',
      editingTicket: null,
      form: {
        title: '',
        assignedTo: '',
        status: 'Open'
      }
    }
  },
  computed: {
    filteredTickets() {
      return this.tickets.filter(ticket => {
        const statusMatch = this.filterStatus === 'All' || ticket.status === this.filterStatus
        const assigneeMatch = !this.filterAssignee || ticket.assignedTo.toLowerCase().includes(this.filterAssignee.toLowerCase())
        return statusMatch && assigneeMatch
      })
    }
  },
  mounted() {
    const session = localStorage.getItem('ticketapp_session')
    if (!session) this.$router.push('/login')

    const savedTickets = localStorage.getItem('ticketapp_tickets')
    if (savedTickets) this.tickets = JSON.parse(savedTickets)
  },
  watch: {
    tickets: {
      handler(newVal) {
        localStorage.setItem('ticketapp_tickets', JSON.stringify(newVal))
      },
      deep: true
    }
  },
  methods: {
    handleSubmit() {
      const { title, assignedTo, status } = this.form

      if (!title || !assignedTo) {
        toast.error('All fields are required')
        return
      }

      if (this.editingTicket) {
        const updated = this.tickets.map(t =>
          t.id === this.editingTicket.id ? { ...t, title, assignedTo, status } : t
        )
        this.tickets = updated
        this.editingTicket = null
        toast.success('Ticket updated')
      } else {
        const newTicket = {
          id: Date.now(),
          title,
          assignedTo,
          status
        }
        this.tickets.push(newTicket)
        toast.success('Ticket created')
      }

      this.form = {
        title: '',
        assignedTo: '',
        status: 'Open'
      }
    },
    editTicket(ticket) {
      this.editingTicket = ticket
      this.form = { ...ticket }
      toast.info('Ticket ready to edit')
    },
    deleteTicket(id) {
      this.tickets = this.tickets.filter(ticket => ticket.id !== id)
      toast.warning('Ticket deleted')
    },
    logout() {
      localStorage.removeItem('ticketapp_session')
      this.$router.push('/login')
    }
  }
}
</script>

<style scoped>
.dashboard-container {
  padding: 2rem;
  max-width: 960px;
  margin: auto;
  font-family: 'Inter', 'Segoe UI', sans-serif;
  color: #333;
}

.dashboard-header h2 {
  text-align: center;
  font-size: 2rem;
  margin-bottom: 0.5rem;
}

.dashboard-header p {
  text-align: center;
  font-size: 1rem;
  color: #666;
}

.filters {
  margin-top: 2rem;
  display: flex;
  gap: 1rem;
  flex-wrap: wrap;
}

.filters select,
.filters input {
  padding: 0.5rem 0.75rem;
  border: 1px solid #ccc;
  border-radius: 6px;
  font-size: 1rem;
  background-color: #fff;
}

.ticket-list {
  margin-top: 2rem;
}

.ticket-list h3 {
  font-size: 1.5rem;
  margin-bottom: 1rem;
}

.ticket-card {
  background-color: #fff;
  border: 1px solid #e0e0e0;
  border-radius: 10px;
  padding: 1rem;
  width: 300px;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.05);
  position: relative;
  transition: transform 0.2s ease;
}

.ticket-card:hover {
  transform: translateY(-2px);
}

.ticket-card h4 {
  margin-bottom: 0.5rem;
  font-size: 1.1rem;
}

.ticket-card p {
  margin: 0.3rem 0;
  font-size: 0.95rem;
}

.ticket-status {
  font-weight: bold;
}

.ticket-status.open {
  color: #007bff;
}

.ticket-status.in-progress {
  color: #ffc107;
}

.ticket-status.closed {
  color: #28a745;
}

.ticket-actions {
  position: absolute;
  top: 10px;
  right: 10px;
  display: flex;
  gap: 0.5rem;
}

.ticket-actions button {
  padding: 0.3rem 0.6rem;
  font-size: 0.8rem;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  color: white;
  transition: background-color 0.2s ease;
}

.ticket-actions .edit {
  background-color: #17a2b8;
}

.ticket-actions .edit:hover {
  background-color: #138496;
}

.ticket-actions .delete {
  background-color: #dc3545;
}

.ticket-actions .delete:hover {
  background-color: #c82333;
}

.ticket-form {
  margin-top: 3rem;
}

.ticket-form h3 {
  font-size: 1.4rem;
  margin-bottom: 1rem;
}

.ticket-form input,
.ticket-form select {
  width: 100%;
  padding: 0.5rem 0.75rem;
  margin-bottom: 1rem;
  border: 1px solid #ccc;
  border-radius: 6px;
  font-size: 1rem;
  background-color: #fff;
}

.ticket-form button {
  padding: 0.75rem 1.5rem;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  color: white;
  font-size: 1rem;
  transition: background-color 0.2s ease;
}

.ticket-form .create {
  background-color: #28a745;
}

.ticket-form .create:hover {
  background-color: #218838;
}

.ticket-form .update {
  background-color: #17a2b8;
}

.ticket-form .update:hover {
  background-color: #138496;
}

.logout-button {
  margin-top: 3rem;
  padding: 0.75rem 1.5rem;
  background-color: #dc3545;
  color: white;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  font-size: 1rem;
  transition: background-color 0.2s ease;
}

.logout-button:hover {
  background-color: #c82333;
}

</style>
