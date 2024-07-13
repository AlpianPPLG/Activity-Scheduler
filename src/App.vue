<template>
  <div class="app">
    <header class="header">
      <h1 class="title">Activity Scheduler</h1>
    </header>
    <main class="main-content">
      <section class="activity-list">
        <h2 class="section-title">Upcoming Activities</h2>
        <ul class="activities">
          <li v-for="activity in activities" :key="activity.id" class="activity">
            <div class="activity-info">
              <span class="activity-name">{{ activity.name }}</span>
              <span class="activity-date">{{ activity.date }}</span>
            </div>
            <div class="activity-actions">
              <button @click="editActivity(activity)" class="action-button edit-button">
                Edit
              </button>
              <button @click="deleteActivity(activity.id)" class="action-button delete-button">
                Delete
              </button>
            </div>
          </li>
        </ul>
      </section>
      <section class="add-activity">
        <h2 class="section-title">Add New Activity</h2>
        <form @submit.prevent="addActivity" class="activity-form">
          <div class="form-group">
            <label for="name" class="label">Activity Name</label>
            <input type="text" v-model="newActivity.name" id="name" class="input" />
          </div>
          <div class="form-group">
            <label for="date" class="label">Date</label>
            <input type="date" v-model="newActivity.date" id="date" class="input" />
          </div>
          <button type="submit" class="button">Add Activity</button>
        </form>
      </section>
    </main>
    <footer class="footer">
      <p>&copy; 2024 Activity Scheduler</p>
    </footer>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      activities: [
        { id: 1, name: 'Meeting with team', date: '2024-07-15' },
        { id: 2, name: 'Project presentation', date: '2024-07-16' }
      ],
      newActivity: {
        name: '',
        date: ''
      }
    }
  },
  methods: {
    addActivity() {
      if (this.newActivity.name.trim() !== '' && this.newActivity.date.trim() !== '') {
        this.activities.push({
          id: Date.now(),
          name: this.newActivity.name,
          date: this.newActivity.date
        })
        this.newActivity.name = ''
        this.newActivity.date = ''
      } else {
        alert('Please fill out both activity name and date.')
      }
    },
    editActivity(activity) {
      const newName = prompt('Enter new name:', activity.name)
      const newDate = prompt('Enter new date (YYYY-MM-DD):', activity.date)
      if (newName !== null && newDate !== null) {
        activity.name = newName
        activity.date = newDate
      }
    },
    deleteActivity(activityId) {
      const confirmed = confirm('Are you sure you want to delete this activity?')
      if (confirmed) {
        this.activities = this.activities.filter((activity) => activity.id !== activityId)
      }
    }
  }
}
</script>

<style>
/* Styles for the entire app */
.app {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
  font-family: Arial, sans-serif;
  background-color: #f0f0f0;
  margin: 0;
  padding: 0;
}

/* Header styles */
.header {
  color: black;
  padding: 20px;
  text-align: center;
}

.title {
  font-size: 2rem;
}

/* Main content styles */
.main-content {
  flex: 1; /* Memanjangkan konten utama untuk mengisi sisa ruang */
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 20px;
}

.section-title {
  font-size: 1.5rem;
  margin-bottom: 10px;
}

/* Activity list styles */
.activity-list {
  width: 100%;
  max-width: 600px;
}

.activities {
  list-style-type: none;
  padding: 0;
}

.activity {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background-color: white;
  padding: 10px;
  margin-bottom: 10px;
  border-radius: 5px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.activity-info {
  flex: 1;
}

.activity-name {
  font-weight: bold;
  margin-right: 10px; /* Jarak antara nama aktivitas dan tanggal */
}

.activity-date {
  color: #666;
}

.activity-actions {
  display: flex;
}

.action-button {
  margin-left: 5px;
  padding: 5px 10px;
  cursor: pointer;
  border: none;
  border-radius: 5px;
}

.edit-button {
  background-color: #38a169;
  color: white;
}

.delete-button {
  background-color: #e53e3e;
  color: white;
}

/* Add activity form styles */
.add-activity {
  width: 100%;
  max-width: 600px;
  margin-top: 20px;
}

.activity-form {
  background-color: white;
  padding: 20px;
  border-radius: 5px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.form-group {
  margin-bottom: 10px;
}

.label {
  display: block;
  margin-bottom: 5px;
}

.input {
  width: 100%;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
}

.button {
  background-color: #3490dc;
  color: white;
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

/* Footer styles */
.footer {
  color: black;
  padding: 20px;
  text-align: center;
  margin-top: auto; /* Menetapkan margin atas auto untuk memposisikan footer di bagian bawah */
}
</style>
