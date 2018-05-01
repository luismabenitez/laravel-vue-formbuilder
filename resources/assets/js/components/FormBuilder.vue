<template>
    <div class="container">
        <div class="row">
            <div class="col-md-6 p-4" style="height:auto;background-color:#ffffff;">
                <h4 class="text-muted text-center mt-5" v-if="!form.length">DRAG FIELDS HERE</h4>
                <form>
                    <div class='form-group' v-for="(field, index) in form">
                        <label class="control-label">{{ field.label }}</label><span class="asterisk" v-if="field.required">*</span>
                        <span class="float-right">
                            <a class="pr-3 field-tools" data-toggle='collapse' :href='"#" + index' role='button' aria-expanded='false'><i class="fas fa-edit"></i></a>
                            <a class="field-tools" href="#" role='button' @click.prevent="clearField(field, index)"><i class="fas fa-trash"></i></a>
                        </span>
                        <template v-if="field.input === 'h2'">
                            <h2>{{ field.text }}</h2>
                        </template>
                        <template v-if="field.input === 'p'">
                            <p>{{ field.textarea }}</p>
                        </template>
                        <template v-if="field.input === 'input'">
                            <input :type="field.type" :name="field.name" :class="field.class" :placeholder="field.placeholder" :required="field.required">
                        </template>
                        <template v-if="field.input === 'textarea'">
                            <textarea :name="field.name" :rows="field.rows" :class="field.class" :placeholder="field.placeholder" :required="field.required"></textarea>
                        </template>
                        <template v-if="field.input === 'button'">
                            <button :type="field.name" :class="field.class">{{ field.text }}</button>
                        </template>
                        <div class='collapse multi-collapse' :id='index'>
                            <div class='card card-body'>
                                <div class="form-group" v-if="field.textarea">
                                    <label class="f10">Paragraph Text</label>
                                    <textarea class="form-control" v-model='field.textarea'>{{ field.textarea }}</textarea>
                                </div>
                                <div class="form-row">
                                    <div class="form-group col-md-6" v-if="field.label">
                                        <label class="f10">Input Label</label>
                                        <input type='text' class='form-control form-control-sm' placeholder='Enter label here'  v-model='field.label' >
                                    </div>
                                    <div class="form-group col-md-6" v-if="field.placeholder">
                                        <label class="f10">Input Placeholder</label>
                                        <input type='text' class='form-control form-control-sm' placeholder='Enter placeholder here' v-model='field.placeholder' >
                                    </div>
                                </div>
                                <div class="form-row">
                                    <div class="form-group col-md-4" v-if="field.type">
                                        <label class="f10">Input Type <span id="example" class="pointer blue" v-on:mouseover="helper" data-toggle="inputType">?</span></label>
                                        <input type='text' class='form-control form-control-sm' placeholder='Enter type here' v-model='field.type' >
                                    </div>
                                    <div class="form-group col-md-4" v-if="field.rows">
                                        <label class="f10">Rows</label>
                                        <input type='text' class='form-control form-control-sm' placeholder='Enter rows here' v-model='field.rows' >
                                    </div>
                                    <div class="form-group col-md-4" v-if="field.text">
                                        <label class="f10">Text</label>
                                        <input type='text' class='form-control form-control-sm' placeholder='Enter text here' v-model='field.text' >
                                    </div>
                                    <div class="form-group col-md-4" v-if="field.name">
                                        <label class="f10">Input Name <span id="example" class="pointer blue" v-on:mouseover="helper" data-toggle="inputName">?</span></label>
                                        <input type='text' class='form-control form-control-sm' placeholder='Enter name here' v-model='field.name' >
                                    </div>
                                    <div class="form-group col-md-4" v-if="field.class">
                                        <label class="f10">Input Class</label>
                                        <input type='text' class='form-control form-control-sm' placeholder='Enter class here' v-model='field.class' >
                                    </div>
                                </div>
                                <div class="form-group form-check" v-if="field.requiredField">
                                    <input type="checkbox" class="form-check-input" id="required" v-model="field.required">
                                    <label class="form-check-label f10" for="required">{{ field.requiredField }}</label>
                                </div>
                            </div>
                        </div>
                    </div>
                </form>
            </div>
            <div class="col-md-4">
                <ul class="list-group">
                    <li class="list-group-item pointer" @click="headerField"><i class="fas fa-arrow-left"></i> <span class="pl-5"><i class="fas fa-heading"></i> Header Field</span></li>
                    <li class="list-group-item pointer" @click="paragraphField"><i class="fas fa-arrow-left"></i> <span class="pl-5"><i class="fas fa-paragraph"></i> Paragraph Field</span></li>
                    <li class="list-group-item pointer" @click="textField"><i class="fas fa-arrow-left"></i> <span class="pl-5"><i class="fas fa-i-cursor"></i> Text Field</span></li>
                    <li class="list-group-item pointer" @click="textareaField"><i class="fas fa-arrow-left"></i> <span class="pl-5"><i class="fas fa-align-center"></i> Textarea Field</span></li>
                    <li class="list-group-item pointer" @click="submitButton"><i class="fas fa-arrow-left"></i> <span class="pl-5"><button class="btn btn-secondary btn-sm"></button> Button</span></li>
                </ul>
                <button class="btn btn-primary btn-sm float-right mt-2" @click.prevent="clearForm"><i class="far fa-times-circle"></i> Clear Form</button>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        data () {
            return {
                form: [],
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
                    class: 'form-control',
                    requiredField: 'required',
                    required: false
                })
            },
            textareaField () {
                this.form.push({
                    input: 'textarea',
                    label: 'Label',
                    placeholder: 'Placeholder',
                    name: 'name',
                    rows: '5',
                    class: 'form-control',
                    requiredField: 'required',
                    required: false
                })
            },
            submitButton () {
                this.form.push({
                    input: 'button',
                    type: 'submit',
                    class: 'btn btn-primary',
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
            clearField (field, index) {
                this.form.splice(index, 1);
            },
            clearForm () {
                this.form = []
            },
            helper () {
                $('[data-toggle="inputType"]').popover({
                    title: 'Types to Choose:',
                    content: 'text, email, number, tel',
                    placement: 'left'
                })
                $('[data-toggle="inputName"]').popover({
                    title: 'Notes',
                    content: 'This field needs to be specific to a database column',
                    placement: 'bottom'
                })
            }
        }
    }
</script>

<style>
    .pointer {
        cursor: pointer;
    }
    .f10 {
        font-size:10px;
    }
    .field-tools {
        color: grey;
    }
    .blue {
        color: #007bff;
    }
    .asterisk {
        color: red;
    }
</style>