<template>

    <div id="pages-container">
        <ul>
            <button v-if="checkPrevBtn()" @click="prevSet">prev</button>
            <li :class="{'selected': pages.current == i}" :key="`pag-${i}`" v-for="(pag, i) in pages.set">
                <button :id="i" @click="sendCurrentPage">{{pag}}</button>
            </li>
            <button v-if="checkNextBtn()" @click="nextSet">next</button>
        </ul>
    </div>

</template>

<script>

export default {
    name: 'pagination',
    props: {
        total: Number,
        limitOfSet: Number,
        limitOfPage: Number
    },
    data(){
        return {
            pages: {
                totalPages: 0,
                set: [],
                current: 0
            }
        }
    },
    watch: {
        total() {
            this.pages.totalPages = Math.ceil(this.total / this.limitOfPage);
            this.pages.set = [];
            this.pages.current = 0;
            this.initSet();
        }
    },
    methods: {
        sendCurrentPage(e) {
            this.pages.current = +e.target.id;
            this.$emit('page', +e.target.innerText - 1);
        },
        sendFirstPage() {
            this.pages.current = 0;
            this.$emit('page', this.pages.set[0]);
        },
        initSet(){
            for(let i = 1; i <= this.limitOfSet; i++) {
                if(i <= this.pages.totalPages){
                    this.pages.set.push(i);
                }else{
                    break;
                }
            }
        },
        prevSet() {
            if(this.pages.set.length < this.limitOfSet){
                this.pages.set = this.pages.set.map(pag => pag - this.limitOfSet);
                let lastPage = this.pages.set.slice(-1)[0];
                for(let i = ++lastPage; this.pages.set.length < this.limitOfSet; ++i ){
                    this.pages.set.push(i);
                }
            }else if(this.pages.set[0] > 1) {
                this.pages.set = this.pages.set.map(pag => pag - this.limitOfSet);
            }
            this.sendFirstPage();
        },
        nextSet() {
            this.pages.set = this.pages.set.filter(val => val + this.limitOfSet <= this.pages.totalPages).map(val => val + this.limitOfSet);
            this.sendFirstPage();
        },
        checkPrevBtn() {
            let firstPage = this.pages.set.slice(0)[0];
            return firstPage != 1 && this.pages.set.length > 0 ? true : false;
        },
        checkNextBtn() {
            let lastPage = this.pages.set.slice(-1)[0];
            return lastPage < this.pages.totalPages && this.pages.set.length > 0 ? true : false;
        }
    }
}

</script>

<style scoped>

    #pages-container {
        box-shadow: 0px 1px 3px 0px rgba(0, 0, 0, 0.58);
        background-color: white;
        height: 50px;
        display: flex;
        justify-content: center;
        align-items: center;
        position: fixed;
        bottom: 0;
        width: 100%;
        color: black
    }

    ul {
        width: 35%;
        display: flex;
        justify-content: center;
    }

    li {
        list-style: none;
        margin: 0 10px;
    }

    .selected button{
        color: #2c3e50;
        font-weight: bold;
        font-size: 1rem;
    }

    button {
        border: none;
        margin: none;
        background: none;
        cursor: pointer;
        padding: 10px;
    }

    @media (max-width: 375px) {
        button {
            padding: 5px;
        }
    }

</style>