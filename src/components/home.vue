<template>
    <div>
        <h1>Welcome {{user.firstName}}!</h1>
        <p>Nice day</p>
        <h3>Users from secure api end point:</h3>
        <em v-if="users.loading">Loading users...</em>
        <span v-if="users.error" class="text-danger">ERROR: {{users.error}}</span>
        <ul v-if="users.items">
            <li v-for="user in users.items" :key="user.id">
                {{user.firstName + ' ' + user.lastName}}
                <span v-if="user.deleting"><em> - Deleting...</em></span>
                <span v-else-if="user.deleteError" class="text-danger"> - ERROR: {{user.deleteError}}</span>
                <span v-else> - <a @click="deleteUser(user.id)" class="text-danger">Delete</a></span>
            </li>
        </ul>
        <p>
            <router-link to="/login">Logout</router-link>
        </p>
    </div>
</template>

<script>
import { mapState, mapActions } from 'vuex'

export default {
    computed: {
        user(){
			return this.$store.state.authentication.user
		},
		users(){
			return this.$store.state.users.all
		}
	},
	methods:{
		...mapActions('users',{
			deleteUser:'delete'
		})
	},
	created(){
		this.$store.dispatch('users/getAll')
	}
};
</script>
