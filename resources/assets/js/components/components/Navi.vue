<template>
    <nav class="navbar navbar-expand-lg nav py-1 px-3">
        <div class="container">
            <h1 class="navbar-brand title">
                <i class="fab fa-github"></i>
                <span>GitGraph</span>
            </h1>
            <button type="button" class="navbar-toggler navIconWrapper" data-toggle="collapse" data-target="#Navber" aria-controls="Navber" aria-expanded="false">
                <div class="navIcon">
                    <div v-for="index in 4" :class="'navIconBox' + index" class="navIconBox"></div>
                </div>
            </button>
            <div class="collapse navbar-collapse" id="Navber">
                <div class="form-inline my-2 my-lg-0">
                    <input type="text" class="form-control mr-sm-2 formName" v-model="name" @keyup.enter="sendName" placeholder="Please enter username" maxlength="39" autofocus>
                    <button type="button" class="btn btn-primary formSend my-2 my-sm-0 mx-1" @click="sendName">Send</button>
                </div>
                <ul class="navbar-nav mr-auto">
                    <li class="nav-item">
                        <a class="nav-link link" href="https://github.com/tyokinuhata/git-graph" target="_blank">GitHub</a>
                    </li>
                    <li class="nav-item">
                        <a class="nav-link link" href="https://twitter.com/h_tyokinuhata" target="_blank">Twitter</a>
                    </li>
                </ul>
            </div>
        </div>
    </nav>
</template>

<script>
    export default {
        data() {
            return {
                name: ''
            }
        },
        methods: {
            sendName() {
                const names = (this.name.trim()).split(' ')
                this.$emit('form-data', {
                    load: 'load',
                    name: names[0]
                })
                axios.post('/api/grasses', {
                    name: names[0]
                })
                    .then(response => {
                        this.$emit('form-grasses', response.data)
                    })
                    .catch(error => {
                        this.$emit('form-grasses', 'You sent a bad request 😔')
                    })
            },
        }
    }
</script>

<style lang="scss" scoped>
    .nav {
        background: #24292e;
    }

    .navIconWrapper {
        outline: none;
    }

    .navIcon {
        width: 20px;
        height: 20px;
        font-size: 0;

        &Box {
            display: inline-block;
            width: 10px;
            height: 10px;
        }

        &Box1 {
            background: rgb(198, 228, 139);
        }

        &Box2 {
            background: rgb(123, 201, 111);
        }

        &Box3 {
            background: rgb(35, 154, 59);
        }

        &Box4 {
            background: rgb(25, 97, 39);;
        }
    }

    .title {
        margin: 0;
        margin-right: 20px;
        padding: 0;
        font-size: 30px;
        color: #fff;
    }

    .formName {
        width: 200px;
        height: 30px;
        box-shadow: none;
        border: 0;
        background: #3f4449;
        color: #fff;

        &:focus {
            background: #4b4f54;
        }
    }

    .formSend {
        margin-right: 20px;
        box-shadow: none;
        border: 0;
        background: #3f4449;

        &:hover {
            background: #4b4f54;
        }
    }

    .link {
        font-weight: 600;
        color: rgba(255,255,255,0.75);

        &:hover {
            color: #fff;
        }
    }
</style>