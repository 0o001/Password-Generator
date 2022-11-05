<template>
    <label for="generate" class="mb-2 text-sm font-medium text-gray-900 sr-only dark:text-gray-300">Check Password</label>
    <div class="relative">
        <input type="password" v-model="passwordText"  @input="checkPasswordStrength" class="block p-4 w-full text-sm text-gray-900 bg-gray-50 rounded-lg border border-gray-300 focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500" placeholder="Write a password">
    </div>
    <div class="flex -mx-1 mt-5">
        <template v-for="(v, i) in checkerCount">
            <div class="w-1/5 px-1">
                <div class="h-2 rounded-xl transition-colors" :class="i<passwordStrength?(passwordStrength<=2?'bg-red-400':(passwordStrength<=4?'bg-yellow-400':'bg-green-500')):'bg-gray-200'"></div>
            </div>
        </template>
    </div>
</template>
<script lang="ts">
export default {
    data() {
        return {
            passwordText: '',
            passwordStrength: 0,
            checkerCount: 5
        }
    },
    methods: {
        checkPasswordStrength() {
            this.passwordStrength = 0
            const checkers: RegExp[] = [
                /[a-z]+/,
                /[A-Z]+/,
                /[0-9]+/,
                /[$@#&!\+-]+/
            ]

            checkers.forEach(item => {
                if(this.passwordText.match(item)) {
                    this.passwordStrength++
                }
            })

            if(this.passwordText.length > 8) {
                this.passwordStrength++
            }
        }
    }
}
</script>