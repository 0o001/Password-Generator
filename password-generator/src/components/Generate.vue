<template>
    <label for="generate" class="mb-2 text-sm font-medium text-gray-900 sr-only dark:text-gray-300">Generate Password</label>
    <div class="relative">
        <input type="text" v-model="passwordText" class="block p-4 pr-[190px] w-full text-sm text-gray-900 bg-gray-50 rounded-lg border border-gray-300 focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500" readonly>
        <button type="button" @click="generate" class="text-white absolute right-2.5 bottom-2.5 bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-lg text-sm px-4 py-2 dark:bg-blue-600 dark:hover:bg-blue-700 dark:focus:ring-blue-800">Generate</button>
        <button type="button" @click="getCopyPassword" class="text-white absolute right-28 bottom-2.5 bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-lg text-sm px-4 py-2 dark:bg-blue-600 dark:hover:bg-blue-700 dark:focus:ring-blue-800">{{ copy ? 'Copied!' : 'Copy' }}</button>
    </div>
    <div id="accordion-flush" data-accordion="collapse" class="mt-5" data-active-classes="bg-white dark:bg-gray-900 text-gray-900 dark:text-white" data-inactive-classes="text-gray-500 dark:text-gray-400">
        <h2 id="accordion-flush-heading-1" @click="open = !open">
            <button type="button" class="flex items-center justify-between w-full py-5 font-medium text-left text-gray-500 border-b border-gray-200 dark:border-gray-700 dark:text-gray-400" data-accordion-target="#accordion-flush-body-1" aria-expanded="true" aria-controls="accordion-flush-body-1">
            <span>Advanced</span>
            <svg data-accordion-icon class="w-6 h-6 rotate-180 shrink-0" fill="currentColor" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg"><path fill-rule="evenodd" d="M5.293 7.293a1 1 0 011.414 0L10 10.586l3.293-3.293a1 1 0 111.414 1.414l-4 4a1 1 0 01-1.414 0l-4-4a1 1 0 010-1.414z" clip-rule="evenodd"></path></svg>
            </button>
        </h2>
        <div id="accordion-flush-body-1" :class="open ? null : 'hidden'" aria-labelledby="accordion-flush-heading-1">
            <div class="flex py-5 font-light">
                <div class="flex-1 items-center p-4 mr-2 rounded border border-gray-200 dark:border-gray-700">
                    <input id="bordered-checkbox-1" type="checkbox" v-model="advanced.upperCase" name="bordered-checkbox" class="w-4 h-4 text-blue-600 bg-gray-100 rounded border-gray-300 focus:ring-blue-500 dark:focus:ring-blue-600 dark:ring-offset-gray-800 focus:ring-2 dark:bg-gray-700 dark:border-gray-600">
                    <label for="bordered-checkbox-1" class="py-4 ml-2 w-full text-sm font-medium text-gray-900 dark:text-gray-300">Uppercase</label>
                </div>
                <div class="flex-1 items-center p-4 rounded border border-gray-200 dark:border-gray-700">
                    <input id="bordered-checkbox-2" type="checkbox" v-model="advanced.lowerCase" name="bordered-checkbox" class="w-4 h-4 text-blue-600 bg-gray-100 rounded border-gray-300 focus:ring-blue-500 dark:focus:ring-blue-600 dark:ring-offset-gray-800 focus:ring-2 dark:bg-gray-700 dark:border-gray-600" disabled>
                    <label for="bordered-checkbox-2" class="py-4 ml-2 w-full text-sm font-medium text-gray-900 dark:text-gray-300">Lowecase</label>
                </div>
            </div>
            <div class="flex py-1 font-light">
                <div class="flex-1 items-center p-4 mr-2 rounded border border-gray-200 dark:border-gray-700">
                    <input id="bordered-checkbox-3" type="checkbox" v-model="advanced.numbers" name="bordered-checkbox" class="w-4 h-4 text-blue-600 bg-gray-100 rounded border-gray-300 focus:ring-blue-500 dark:focus:ring-blue-600 dark:ring-offset-gray-800 focus:ring-2 dark:bg-gray-700 dark:border-gray-600">
                    <label for="bordered-checkbox-3" class="py-4 ml-2 w-full text-sm font-medium text-gray-900 dark:text-gray-300">Numbers</label>
                </div>
                <div class="flex-1 items-center p-4 rounded border border-gray-200 dark:border-gray-700">
                    <input id="bordered-checkbox-4" type="checkbox" v-model="advanced.symbols" name="bordered-checkbox" class="w-4 h-4 text-blue-600 bg-gray-100 rounded border-gray-300 focus:ring-blue-500 dark:focus:ring-blue-600 dark:ring-offset-gray-800 focus:ring-2 dark:bg-gray-700 dark:border-gray-600">
                    <label for="bordered-checkbox-4" class="py-4 ml-2 w-full text-sm font-medium text-gray-900 dark:text-gray-300">Symbols</label>
                </div>
            </div>
            <div class="flex py-5 font-light">
                <div class="flex-1 items-center p-4 rounded border border-gray-200 dark:border-gray-700">
                    <label for="default-range" class="block mb-2 text-sm font-medium text-gray-900 dark:text-gray-300">Password Length: {{ advanced.passwordLength }}</label>
                    <input id="default-range" type="range" v-model="advanced.passwordLength" max="50" class="w-full h-2 bg-gray-200 rounded-lg appearance-none cursor-pointer dark:bg-gray-700">
                </div>
            </div>
        </div>
    </div>
</template>

<script lang="ts">
export default {
    data() {
        return {
            open: false,
            passwordText: '',
            copy: false,
            advanced: {
                upperCase: true,
                lowerCase: true,
                numbers: true,
                symbols: true,
                passwordLength: 20
            }
        }
    },
    methods: {
        getCopyPassword() {
            navigator.clipboard
            .writeText(this.passwordText)
            .then(() => {
                this.copy = true
                setTimeout(() => {
                    this.copy = false
                }, 200)
            })
            .catch(() => alert("Error! Something went wrong."))
        },
        generate() {
            const defaultCharacters = 'abcdefghijklmnopqrstuvwxyz'
            const characters = {
                lowerCase: defaultCharacters,
                upperCase: defaultCharacters.toUpperCase(),
                numbers: '0123456789',
                symbols: '~!@-#$+-*'
            }

            const characterList = [
                ...this.advanced.lowerCase ? characters.lowerCase : [],
                ...this.advanced.upperCase ? characters.upperCase : [],
                ...this.advanced.numbers ? characters.numbers : [],
                ...this.advanced.symbols ? characters.symbols : []
            ].join('')

            this.passwordText = Array.from({ length: this.advanced.passwordLength }, () => Math.floor(Math.random() * characterList.length))
            .map(number => characterList[number])
            .join('')
        }
    },
    mounted() {
        this.generate()
    }
}
</script>