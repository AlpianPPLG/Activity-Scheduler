<template>
  <div class="app" :class="{ dark: isDarkMode }">
    <header class="header">
      <h1 class="title">Activity Scheduler</h1>
      <button @click="toggleTheme" class="theme-toggle">
        {{ isDarkMode ? 'Light Mode' : 'Dark Mode' }}
      </button>
    </header>
    <main class="main-content">
      <section class="filter-sort">
        <input type="text" v-model="filterText" placeholder="Filter by name" class="filter-input" />
        <select v-model="sortOption" @change="sortActivities" class="sort-select">
          <option value="name">Sort by Name</option>
          <option value="date">Sort by Date</option>
        </select>
        <select v-model="filterCategory" class="filter-select">
          <option value="">All Categories</option>
          <option v-for="category in categories" :key="category" :value="category">
            {{ category }}
          </option>
        </select>
      </section>
      <section class="activity-list">
        <h2 class="section-title">Upcoming Activities</h2>
        <ul class="activities">
          <li v-for="activity in filteredActivities" :key="activity.id" class="activity">
            <div class="activity-info">
              <span class="activity-name">{{ activity.name }}</span>
              <span class="activity-date">{{ activity.date }}</span>
              <span class="activity-category">{{ activity.category }}</span>
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
          <div class="form-group">
            <label for="category" class="label">Category</label>
            <select v-model="newActivity.category" id="category" class="input">
              <option value="" disabled>Select a category</option>
              <option v-for="category in categories" :key="category" :value="category">
                {{ category }}
              </option>
            </select>
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
      activities: JSON.parse(localStorage.getItem('activities')) || [],
      newActivity: {
        name: '',
        date: '',
        category: ''
      },
      filterText: '',
      sortOption: 'name',
      filterCategory: '',
      categories: ['Work', 'Personal', 'Health', 'Other'], // Tambahkan kategori di sini
      isDarkMode: false
    }
  },
  computed: {
    filteredActivities() {
      return this.activities
        .filter(
          (activity) =>
            activity.name.toLowerCase().includes(this.filterText.toLowerCase()) &&
            (this.filterCategory === '' || activity.category === this.filterCategory)
        )
        .sort((a, b) => {
          if (this.sortOption === 'date') {
            return new Date(a.date) - new Date(b.date)
          } else {
            return a.name.localeCompare(b.name)
          }
        })
    }
  },
  methods: {
    addActivity() {
      if (
        this.newActivity.name.trim() !== '' &&
        this.newActivity.date.trim() !== '' &&
        this.newActivity.category
      ) {
        const newActivity = {
          id: Date.now(),
          name: this.newActivity.name,
          date: this.newActivity.date,
          category: this.newActivity.category
        }
        this.activities.push(newActivity)
        this.saveToLocalStorage()
        this.newActivity.name = ''
        this.newActivity.date = ''
        this.newActivity.category = ''
      } else {
        alert('Please fill out all fields: name, date, and category.')
      }
    },
    editActivity(activity) {
      const newName = prompt('Enter new name:', activity.name)
      const newDate = prompt('Enter new date (YYYY-MM-DD):', activity.date)
      const newCategory = prompt('Enter new category:', activity.category)
      if (newName !== null && newDate !== null && newCategory !== null) {
        activity.name = newName
        activity.date = newDate
        activity.category = newCategory
        this.saveToLocalStorage()
      }
    },
    deleteActivity(activityId) {
      const confirmed = confirm('Are you sure you want to delete this activity?')
      if (confirmed) {
        this.activities = this.activities.filter((activity) => activity.id !== activityId)
        this.saveToLocalStorage()
      }
    },
    saveToLocalStorage() {
      localStorage.setItem('activities', JSON.stringify(this.activities))
    },
    toggleTheme() {
      this.isDarkMode = !this.isDarkMode
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
  transition: background-color 0.3s;
}

.app.dark {
  background-color: #333;
  color: white;
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

.theme-toggle {
  margin-top: 10px;
  padding: 5px 10px;
  cursor: pointer;
}

/* Main content styles */
.main-content {
  flex: 1;
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 20px;
}

/* Filter and Sort styles */
.filter-sort {
  width: 100%;
  max-width: 600px;
  display: flex;
  justify-content: space-between;
  margin-bottom: 20px;
}

.filter-input {
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
  flex: 1;
  margin-right: 10px;
}

.sort-select,
.filter-select {
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
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
  margin-right: 10px;
}

.activity-date {
  color: #666;
}

.activity-category {
  color: #888;
  margin-left: 10px;
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
  margin-top: auto;
}
</style>
