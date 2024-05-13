<template>
    <div class="users">
        <h2>User List</h2>
        <ul>
            <li>
                <ul>
                    <li>First name</li>
                    <li>Last name</li>
                    <li>Email</li>
                    <li>Notification enabled</li>
                    <li>Dashboard enabled</li>
                    <li>Timezone</li>
                </ul>
            </li>
            <li v-for="user in users" :key="user.id">
                <ul>
                    <li>{{ user.firstName }}</li>
                    <li>{{ user.lastName }}</li>
                    <li>{{ user.email }}</li>
                    <li>{{ user.settings.isNotificationEnabled }}</li>
                    <li>{{ user.settings.isNewDashboardEnabled }}</li>
                    <li>{{ user.settings.timezone }}</li>
                </ul>
            </li>
        </ul>
        <nav>
            <button @click="previousPage" :disabled="currentPage === 1">Previous</button>
            <span>Page {{ currentPage }} of {{ totalPages }}</span>
            <button @click="nextPage" :disabled="currentPage === totalPages">Next</button>
        </nav>
    </div>
</template>

<script>
export default {
    name: 'UserList',
    data() {
        return {
            users: [],
            currentPage: 1,
            pageSize: 5,
            totalPages: 0
        };
    },
    mounted() {
        this.fetchUsers();
    },
    methods: {
        fetchUsers() {
            const apiUrl = `${process.env.VUE_APP_API_URL}/user?page=${this.currentPage}&limit=${this.pageSize}`;
            fetch(apiUrl)
                .then(response => response.json())
                .then(data => {
                    const { users, count } = data
                    this.users = users;
                    this.totalPages = Math.ceil(count / this.pageSize);
                })
                .catch(error => {
                    console.error('Error fetching user data:', error);
                });
        },
        nextPage() {
            if (this.currentPage < this.totalPages) {
                this.currentPage++;
            }
            this.fetchUsers();
        },
        previousPage() {
            if (this.currentPage > 1) {
                this.currentPage--;
            }
            this.fetchUsers();
        }
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
    margin: 40px 0 0;
}

ul {
    list-style-type: none;
    padding: 0;
}

li {
    display: block;
    margin: 0 10px;
}

li ul li {
    display: inline-block;
    text-align: left;
    width: 150px;
}

a {
    color: #42b983;
}
</style>