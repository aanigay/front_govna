<template>
    <v-sheet width="300" class="mx-auto">

        <v-text-field
                v-model="name"
                label="Username"
                :rules="[loginRules.required]"
        ></v-text-field>

        <v-text-field
                type="password"
                v-model="password"
                label="Password"
                :rules="[passwordRules.required]"
        ></v-text-field>

        <v-btn :disabled="!name || !password" type="submit" block class="mt-2" @submit.prevent @click="onLogin">Login
        </v-btn>
        <v-btn :disabled="!name || !password" type="submit" color="#80CBC4" block class="mt-2" @submit.prevent
               @click="onRegister">Register
        </v-btn>

        <v-dialog
                v-model="dialog"
                width="auto"
        >
            <v-card>
                <v-card-text>
                    Incorrect login or password.
                </v-card-text>
                <v-card-actions>
                    <v-btn color="primary" block @click="dialog = false">Got it</v-btn>
                </v-card-actions>
            </v-card>
        </v-dialog>
    </v-sheet>

</template>

<script>
export default {
    data: () => ({
        name: '',
        password: '',
        dialog: false,
        loginRules: {
            required: value => {
                if (value) return true

                return 'You must enter login.'
            },
        },
        passwordRules: {
            required: value => {
                if (value) return true

                return 'You must enter password.'
            },
        }
    }),
    methods: {
        onLogin() {
            this.$store.dispatch('authModule/onLogin', {
                name: this.name,
                password: this.password
            }).then((res) => {
                console.log('resSSS', res)
                if (!res) {
                    this.dialog = true
                } else {
                    this.$router.push('/feed');
                }
            })
        },
        onRegister() {
            this.$store.dispatch('authModule/onRegister', {
                name: this.name,
                password: this.password
            }).then(() => {
                this.$router.push('/chat');
            })
        }
    }
}
</script>

<style scoped>

</style>