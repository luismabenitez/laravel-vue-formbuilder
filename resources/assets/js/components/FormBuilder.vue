<template>
    <div class="max-w-2xl mx-auto mt-20">
        <h1 class="text-center pb-6 text-2xl text-gray-800 font-semibold">Vue FormBuilder</h1>
        <div class="flex mt-10">
            <div class="bg-white h-auto rounded-lg shadow-lg p-3 w-2/3 mr-3">
                <h4 class="text-muted text-center mt-5" v-if="!form.length">CLICK A FIELD TO ADD IT HERE</h4>
                <form>
                    <div class='mt-4' v-for="(field, index) in form" :key="index">
                        <label class="text-sm font-medium text-gray-700">{{ field.label }}</label><span class="asterisk" v-if="field.required">*</span>
                        <span class="float-right">
                            <a class="pr-3 text-xs text-gray-400" @click.prevent="toggleMenu(index)" role='button' aria-expanded='false'><i class="fas fa-edit"></i></a>
                            <a class="text-xs text-gray-400" href="#" role='button' @click.prevent="clearField(field, index)"><i class="fas fa-trash"></i></a>
                        </span>
                        <template v-if="field.input === 'h2'">
                            <h2>{{ field.text }}</h2>
                        </template>
                        <template v-if="field.input === 'p'">
                            <p>{{ field.textarea }}</p>
                        </template>
                        <template v-if="field.input === 'input'">
                            <input :type="field.type" :name="field.name" :class="field.class" :placeholder="field.placeholder" :required="field.required" :autocomplete="field.autocomplete">
                        </template>
                        <template v-if="field.input === 'textarea'">
                            <textarea :name="field.name" :rows="field.rows" :class="field.class" :placeholder="field.placeholder" :required="field.required"></textarea>
                        </template>
                        <template v-if="field.input === 'button'">
                            <button :type="field.name" :class="field.class">{{ field.text }}</button>
                        </template>
                        <div v-if="toggleMenuId === index">
                            <div class='card card-body'>
                                <div class="mt-2" v-if="field.textarea">
                                    <label class="f10">Paragraph Text</label>
                                    <textarea class="py-1 px-4 w-full border border-gray-300 rounded-md focus:outline-none focus:ring focus:border-blue-300" v-model='field.textarea'></textarea>
                                </div>
                                <div class="form-row">
                                    <div class="mt-2 col-md-6" v-if="field.label">
                                        <label class="f10">Input Label</label>
                                        <input type='text' class='py-1 px-4 w-full border border-gray-300 rounded-md focus:outline-none focus:ring focus:border-blue-300' placeholder='Enter label here'  v-model='field.label' >
                                    </div>
                                    <div class="mt-2 col-md-6" v-if="field.placeholder">
                                        <label class="f10">Input Placeholder</label>
                                        <input type='text' class='py-1 px-4 w-full border border-gray-300 rounded-md focus:outline-none focus:ring focus:border-blue-300' placeholder='Enter placeholder here' v-model='field.placeholder' >
                                    </div>
                                </div>
                                <div class="form-row">
                                    <div class="mt-2 col-md-4" v-if="field.type">
                                        <label class="f10">Input Type</label>
                                        <input type='text' class='py-1 px-4 w-full border border-gray-300 rounded-md focus:outline-none focus:ring focus:border-blue-300' placeholder='Enter type here' v-model='field.type' >
                                    </div>
                                    <div class="mt-2 col-md-4" v-if="field.rows">
                                        <label class="f10">Rows</label>
                                        <input type='text' class='py-1 px-4 w-full border border-gray-300 rounded-md focus:outline-none focus:ring focus:border-blue-300' placeholder='Enter rows here' v-model='field.rows' >
                                    </div>
                                    <div class="mt-2 col-md-4" v-if="field.text">
                                        <label class="f10">Text</label>
                                        <input type='text' class='py-1 px-4 w-full border border-gray-300 rounded-md focus:outline-none focus:ring focus:border-blue-300' placeholder='Enter text here' v-model='field.text' >
                                    </div>
                                    <div class="mt-2 col-md-4" v-if="field.name">
                                        <label class="f10">Input Name</label>
                                        <input type='text' class='py-1 px-4 w-full border border-gray-300 rounded-md focus:outline-none focus:ring focus:border-blue-300' placeholder='Enter name here' v-model='field.name' >
                                    </div>
                                    <div class="mt-2 col-md-4" v-if="field.class">
                                        <label class="f10">Input Class</label>
                                        <input type='text' class='py-1 px-4 w-full border border-gray-300 rounded-md focus:outline-none focus:ring focus:border-blue-300' placeholder='Enter class here' v-model='field.class' >
                                    </div>
                                </div>
                                <div class="mt-2 form-check" v-if="field.requiredField">
                                    <input type="checkbox" class="form-check-input" id="required" v-model="field.required">
                                    <label class="form-check-label f10" for="required">{{ field.requiredField }}</label>
                                </div>
                            </div>
                        </div>
                    </div>
                </form>
            </div>
            <div class="flex flex-col w-1/3">
                <ul class="rounded-lg overflow-hidden shadow">
                    <li class="bg-white px-2 py-2 shadow cursor-pointer" @click="headerField"><i class="fas fa-arrow-left"></i> <span class="pl-5"><i class="fas fa-heading"></i> Header Field</span></li>
                    <li class="bg-white px-2 py-2 shadow cursor-pointer" @click="paragraphField"><i class="fas fa-arrow-left"></i> <span class="pl-5"><i class="fas fa-paragraph"></i> Paragraph Field</span></li>
                    <li class="bg-white px-2 py-2 shadow cursor-pointer" @click="textField"><i class="fas fa-arrow-left"></i> <span class="pl-5"><i class="fas fa-i-cursor"></i> Text Field</span></li>
                    <li class="bg-white px-2 py-2 shadow cursor-pointer" @click="textareaField"><i class="fas fa-arrow-left"></i> <span class="pl-5"><i class="fas fa-align-center"></i> Textarea Field</span></li>
                    <li class="bg-white px-2 py-2 shadow cursor-pointer" @click="submitButton"><i class="fas fa-arrow-left"></i> <span class="pl-5"><button class="btn btn-secondary btn-sm"></button> Button</span></li>
                </ul>
                <button class="bg-blue-200 px-2 py-2 rounded-lg shadow mt-2 focus:outline-none hover:bg-blue-300" @click.prevent="clearForm"><i class="far fa-times-circle"></i> Clear Form</button>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        data () {
            return {
                form: [],
                toggleMenuId: ''
            }
        },
        methods: {
            textField () {
                this.form.push({
                    input: 'input',
                    label: 'Label',
                    placeholder: 'Placeholder',
                    type: 'text',
                    name: 'name',
                    class: 'py-1 px-4 w-full border border-gray-300 rounded-md focus:outline-none focus:ring focus:border-blue-300',
                    requiredField: 'required',
                    required: false,
                    autocomplete: 'off'
                })
            },
            textareaField () {
                this.form.push({
                    input: 'textarea',
                    label: 'Label',
                    placeholder: 'Placeholder',
                    name: 'name',
                    rows: '5',
                    class: 'py-1 px-4 w-full border border-gray-300 rounded-md focus:outline-none focus:ring focus:border-blue-300',
                    requiredField: 'required',
                    required: false
                })
            },
            submitButton () {
                this.form.push({
                    input: 'button',
                    type: 'submit',
                    class: 'bg-blue-200 px-4 py-1 rounded-lg shadow mt-2 focus:outline-none hover:bg-blue-300',
                    text: 'Enter Text'
                })
            },
            headerField () {
                this.form.push({
                    input: 'h2',
                    type: 'submit',
                    text: 'Header'
                })
            },
            paragraphField () {
                this.form.push({
                    input: 'p',
                    textarea: 'Paragraph'
                })
            },
            toggleMenu (index) {
                if (!this.toggleMenuId) {
                    this.toggleMenuId = index
                } else {
                    this.toggleMenuId = ''
                }

            },
            clearField (field, index) {
                this.form.splice(index, 1);
            },
            clearForm () {
                this.form = []
            }
        }
    }
</script>