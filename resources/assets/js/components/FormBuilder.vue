<template>
    <div class="container">
        <div class="row">
            <div class="col-md-6 p-4" style="height:auto;background-color:#ffffff;">
                <form>
                    <div class='form-group' v-for="field in form" v-html="field"></div>
                </form>
            </div>
            <div class="col-md-4">
                <ul class="list-group">
                    <li class="list-group-item pointer" @click="firstNameField"><i class="fas fa-arrow-left"></i> <span class="pl-5"><i class="fas fa-user"></i> First Name</span></li>
                    <li class="list-group-item pointer" @click="lastNameField"><i class="fas fa-arrow-left"></i> <span class="pl-5"><i class="fas fa-user"></i> Last Name</span></li>
                    <li class="list-group-item pointer" @click="phoneField"><i class="fas fa-arrow-left"></i> <span class="pl-5"><i class="fas fa-phone"></i> Phone Number</span></li>
                    <li class="list-group-item pointer" @click="emailField"><i class="fas fa-arrow-left"></i> <span class="pl-5"><i class="fas fa-envelope"></i> Email Address</span></li>
                    <li class="list-group-item pointer" @click="messageField"><i class="fas fa-arrow-left"></i> <span class="pl-5"><i class="fas fa-comment-alt"></i> Message</span></li>
                    <li class="list-group-item pointer" @click="submitButton"><i class="fas fa-arrow-left"></i> <span class="pl-5"><i class="fas fa-save"></i> Button</span></li>
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
                form: []
            }
        },
        methods: {
            firstNameField () {
                let html = "<label class='control-label'>First Name</label><input type='text' placeholder='First Name' class='form-control' name='first'>"
                this.form.push(html)
            },
            lastNameField () {
                let html = "<label class='control-label'>Last Name</label><input type='text' placeholder='Last Name' class='form-control' name='last'>"
                this.form.push(html)
            },
            phoneField () {
                let html = "<label class='control-label'>Phone Number</label><input type='tel' placeholder='Phone Number' class='form-control' name='phone'>"
                this.form.push(html)
            },
            emailField () {
                let html = "<label class='control-label'>Email</label><input type='email' placeholder='Email' class='form-control' name='email'>"
                this.form.push(html)
            },
            messageField () {
                let html = "<label class='control-label'>Message</label><textarea placeholder='Message' class='form-control' name='message' rows='8'></textarea>"
                this.form.push(html)
            },
            submitButton () {
                let html = "<button class='btn btn-primary'>Submit</button>"
                this.form.push(html)
            },
            clearForm () {
                this.form = []
            }
        }
    }
</script>

<style>
    .pointer {
        cursor: pointer;
    }
</style>