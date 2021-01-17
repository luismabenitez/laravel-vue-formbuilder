<template>
    <div class="max-w-4xl mx-auto mt-20">
        <h1 class="pb-6 text-2xl font-semibold text-center text-gray-800">Vue FormBuilder</h1>
        <div class="flex mt-10 mb-20">
            <div class="w-2/3 h-auto mr-3 bg-white rounded-lg shadow-lg">
                <h4 class="mt-5 text-center text-muted" v-if="!form.length">CLICK A FIELD TO ADD IT HERE</h4>
                <form>
                    <div @mouseover="displayOptions = index" @mouseleave="displayOptions = ''" class='px-3 py-2 hover:bg-gray-200' v-for="(field, index) in form" :key="index">
                        <label class="text-sm font-medium text-gray-700">{{ field.label }}</label><span class="text-red-600" v-if="field.required">*</span>
                        <span v-if="displayOptions === index" class="float-right">
                            <a class="pr-3 text-xs text-gray-400" @click.prevent="toggleMenu(index)" role='button' aria-expanded='false'><i class="fas fa-edit"></i></a>
                            <a class="text-xs text-gray-400" href="#" role='button' @click.prevent="clearField(field, index)"><i class="fas fa-trash"></i></a>
                        </span>
                        <template v-if="field.input === 'header'">
                            <h1 v-if="!field.type || field.type === 'h1'" class="text-2xl font-bold">{{ field.text }}</h1>
                            <h2 v-if="field.type === 'h2'" class="text-xl font-bold">{{ field.text }}</h2>
                            <h3 v-if="field.type === 'h3'" class="text-lg font-bold">{{ field.text }}</h3>
                            <h4 v-if="field.type === 'h4'" class="font-bold text-md">{{ field.text }}</h4>
                        </template>
                        <template v-if="field.input === 'p'">
                            <p :class="field.class">{{ field.textarea }}</p>
                        </template>
                        <template v-if="field.input === 'select'">
                            <select :name="field.name" :class="field.class" :required="field.required" :multiple="field.multiple">
                                <option v-for="(option, index) in field.options" :key="index" :value="option.value">{{ option.text }}</option>
                            </select>
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
                            <div class='px-3 py-2 mt-2 bg-gray-300 rounded-lg'>
                                <div class="flex items-center mt-2" v-if="field.textarea">
                                    <label class="w-1/3">Text</label>
                                    <textarea class="w-2/3 px-4 py-1 border border-gray-300 rounded-md focus:outline-none focus:ring focus:border-blue-300" v-model='field.textarea'></textarea>
                                </div>
                                <div class="form-row">
                                    <div class="flex items-center mt-2" v-if="field.label">
                                        <label class="w-1/3">Label</label>
                                        <input type='text' class='w-2/3 px-4 py-1 border border-gray-300 rounded-md focus:outline-none focus:ring focus:border-blue-300' placeholder='Enter label here'  v-model='field.label' >
                                    </div>
                                    <div class="flex items-center mt-2" v-if="field.placeholder">
                                        <label class="w-1/3">Placeholder</label>
                                        <input type='text' class='w-2/3 px-4 py-1 border border-gray-300 rounded-md focus:outline-none focus:ring focus:border-blue-300' placeholder='Enter placeholder here' v-model='field.placeholder' >
                                    </div>
                                </div>
                                <div class="form-row">
                                    <div class="flex items-center mt-2" v-if="field.options">
                                        <label class="w-1/3">Opciones</label>
                                        <textarea class="w-2/3 px-4 py-1 border border-gray-300 rounded-md focus:outline-none focus:ring focus:border-blue-300" v-model='field.optionsPreFormat' v-on:change="parseOptions(field)" placeholder="value=option (una opción por línea)"></textarea>
                                    </div>
                                </div>
                                <div class="form-row">
                                    <div class="flex mt-2" v-if="field.types">
                                        <label class="w-1/3">Type</label>
                                        <select class='w-2/3 px-4 py-1 border border-gray-300 rounded-md focus:outline-none focus:ring focus:border-blue-300' v-model='field.type'>
                                            <option value="">Choose type:</option>
                                            <option v-for="(type, index) in field.types" :key="index" :value="type">{{ type }}</option>
                                        </select>
                                    </div>
                                    <div class="flex mt-2" v-if="field.rows">
                                        <label class="w-1/3">Rows</label>
                                        <input type='text' class='w-2/3 px-4 py-1 border border-gray-300 rounded-md focus:outline-none focus:ring focus:border-blue-300' placeholder='Enter rows here' v-model='field.rows' >
                                    </div>
                                    <div class="flex mt-2" v-if="field.text">
                                        <label class="w-1/3">Text</label>
                                        <input type='text' class='w-2/3 px-4 py-1 border border-gray-300 rounded-md focus:outline-none focus:ring focus:border-blue-300' placeholder='Enter text here' v-model='field.text' >
                                    </div>
                                    <div class="flex mt-2" v-if="field.name">
                                        <label class="w-1/3">Name</label>
                                        <input type='text' class='w-2/3 px-4 py-1 border border-gray-300 rounded-md focus:outline-none focus:ring focus:border-blue-300' placeholder='Enter name here' v-model='field.name' >
                                    </div>
                                    <div class="flex mt-2" v-if="field.class">
                                        <label class="w-1/3">Class</label>
                                        <input type='text' class='w-2/3 px-4 py-1 border border-gray-300 rounded-md focus:outline-none focus:ring focus:border-blue-300' placeholder='Enter class here' v-model='field.class' >
                                    </div>
                                </div>
                                <div class="flex justify-around">
                                    <div class="flex items-center mt-2" v-if="field.requiredField">
                                        <input type="checkbox" id="required" v-model="field.required">
                                        <label for="required" class="ml-3">{{ field.requiredField }}</label>
                                    </div>
                                    <div class="flex items-center mt-2" v-if="field.multipleField">
                                        <input type="checkbox" id="multiple" v-model="field.multiple">
                                        <label for="multiple" class="ml-3">{{ field.multipleField }}</label>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </form>
            </div>
            <div class="flex flex-col w-1/3">
                <ul class="overflow-hidden rounded-lg shadow">
                    <li class="flex items-center px-2 py-2 bg-white shadow cursor-pointer hover:bg-gray-200" @click="headerField">
                        <i class="w-1/6 text-gray-400 fas fa-arrow-left"></i>
                        <i class="w-1/6 text-center fas fa-heading"></i>
                        <p class="w-full">Header Field</p>
                    </li>
                    <li class="flex items-center px-2 py-2 bg-white shadow cursor-pointer hover:bg-gray-200" @click="paragraphField">
                        <i class="w-1/6 text-gray-400 fas fa-arrow-left"></i>
                        <i class="w-1/6 text-center fas fa-paragraph"></i>
                        <p class="w-full">Paragraph Field</p>
                    </li>
                    <li class="flex items-center px-2 py-2 bg-white shadow cursor-pointer hover:bg-gray-200" @click="textField">
                        <i class="w-1/6 text-gray-400 fas fa-arrow-left"></i>
                        <i class="w-1/6 text-center fas fa-i-cursor"></i>
                        <p class="w-full">Text Field</p>
                    </li>
                    <li class="flex items-center px-2 py-2 bg-white shadow cursor-pointer hover:bg-gray-200" @click="textareaField">
                        <i class="w-1/6 text-gray-400 fas fa-arrow-left"></i>
                        <i class="w-1/6 text-center fas fa-align-center"></i>
                        <p class="w-full">Textarea Field</p>
                    </li>
                    <li class="flex items-center px-2 py-2 bg-white shadow cursor-pointer hover:bg-gray-200" @click="selectField">
                        <i class="w-1/6 text-gray-400 fas fa-arrow-left"></i>
                        <i class="w-1/6 text-center fas fa-list-ul"></i>
                        <p class="w-full">Select</p>
                    </li>
                    <li class="flex items-center px-2 py-2 bg-white shadow cursor-pointer hover:bg-gray-200" @click="textareaField">
                        <i class="w-1/6 text-gray-400 fas fa-arrow-left"></i>
                        <i class="w-1/6 text-center far fa-check-square"></i>
                        <p class="w-full">Checkboxes</p>
                    </li>
                    <li class="flex items-center px-2 py-2 bg-white shadow cursor-pointer hover:bg-gray-200" @click="textareaField">
                        <i class="w-1/6 text-gray-400 fas fa-arrow-left"></i>
                        <i class="w-1/6 text-center far fa-check-circle"></i>
                        <p class="w-full">Radio Buttons</p>
                    </li>
                    <li class="flex items-center px-2 py-2 bg-white shadow cursor-pointer hover:bg-gray-200" @click="submitButton">
                        <i class="w-1/6 text-gray-400 fas fa-arrow-left"></i>
                        <i class="w-1/6 text-center far fa-hand-pointer"></i>
                        <p class="w-full">Button</p>
                    </li>
                </ul>
                <button class="px-2 py-2 mt-2 bg-blue-200 rounded-lg shadow focus:outline-none hover:bg-blue-300" @click.prevent="clearForm"><i class="far fa-times-circle"></i> Clear Form</button>
            </div>
        </div>
        <pre class="text-xs">{{ JSON.stringify(form, null, '\t') }}</pre>
    </div>
</template>

<script>
    export default {
        data () {
            return {
                form: [],
                toggleMenuId: '',
                displayOptions: ''
            }
        },
        methods: {
            textField () {
                this.form.push({
                    input: 'input',
                    label: 'Label',
                    placeholder: 'Placeholder',
                    types: ['text', 'email', 'tel', 'date', 'number'],
                    type: 'text',
                    name: 'text-' + Math.floor(100000 + Math.random() * 900000),
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
                    name: 'textarea-' + + Math.floor(100000 + Math.random() * 900000),
                    rows: '5',
                    class: 'py-1 px-4 w-full border border-gray-300 rounded-md focus:outline-none focus:ring focus:border-blue-300',
                    requiredField: 'required',
                    required: false
                })
            },
            submitButton () {
                this.form.push({
                    input: 'button',
                    types: ['button', 'submit'],
                    type: 'submit',
                    class: 'bg-blue-200 px-4 py-2 rounded shadow mt-2 focus:outline-none hover:bg-blue-300',
                    text: 'Submit'
                })
            },
            headerField () {
                this.form.push({
                    input: 'header',
                    types: ['h1', 'h2', 'h3', 'h4'],
                    type: 'h1',
                    text: 'Header',
                    class: 'text-gray-900',
                })
            },
            paragraphField () {
                this.form.push({
                    input: 'p',
                    textarea: 'Paragraph',
                    class: 'text-gray-700',
                })
            },
            selectField () {
                this.form.push({
                    label: 'Label',
                    input: 'select',
                    class: 'py-1 px-4 w-full border border-gray-300 rounded-md focus:outline-none focus:ring focus:border-blue-300',
                    multiple: false,
                    multipleField: 'multiple',
                    name: 'text-' + Math.floor(100000 + Math.random() * 900000),
                    requiredField: 'required',
                    required: false,
                    options: [],
                })
            },
            parseOptions(field){
                var lines = field.optionsPreFormat.split('\n');
                for(var i = 0;i < lines.length;i++){
                    var line = lines[i].split('=')
                    if (line.length == 2){
                        field.options.push({value: line[0], text: line[1]});
                    }
                }
            },
            toggleMenu (index) {
                if(this.toggleMenuId !== index) {
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
            },
        }
    }
</script>
