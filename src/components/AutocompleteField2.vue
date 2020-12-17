<template>
  <div class="autocomplete" ref="container">
        <div class="autocomplete-input__wrap">
            <i class="material-icons">insert_chart</i>
            <input 
                class="autocomplete-input__input" 
                type="text"
                @input="handleInput"
                :value="searchField"
                @keydown="moveByItems"
                @keyup.enter="choiseItem"
            >
        </div>
        <ul class="autocomplete-list" v-if="hasLists">
            <li 
                v-for="(item, idx) in autocomplete.lists" 
                :key="idx"
                class="autocomplete-items"
                :class="{'autocomplete-items_hover': idx === activeItem}"
                :ref="idx === activeItem ? 'active-items' : ''"
                @click="choiseItem(idx)"
            >
                {{item}}
            </li>
        </ul>
  </div>
</template>

<script>


export default {
    name: 'AutocompleteField2',
    props: ['searchInput'],
    data: () => ({
        testlists: [ 'ставрово', 'москва', 'владимир', '123', '123', '123', '123', '123', '222', '333', '123', '123', '123', '123', '123', '222', '333' ],
        searchField: '',
        activeItemIdx: false,
        autocomplete: {
            lists: [ ]
        }
    }),
    watch: {
        searchInput(newVal) {
            this.searchField = newVal
        }
    },
    methods: {
        fetchData() {
            if (this.searchField.trim().length > 2) {
                this.autocomplete.lists = this.testlists
            } else if (this.searchField.trim() === '' || this.searchField.trim().length < 2){
                this.clearList()
                return
            }
            
        },
        handleInput(e) {
            this.searchField = e.target.value
            this.fetchData()
        },
        choiseItem(idx = false) {
            if (idx >= 0) { //по клику мыши 
                this.searchField = this.autocomplete.lists[idx]
                this.$emit('choice', this.autocomplete.lists[idx])
                this.clearList()
            } else { //по нажатию enter
                if (this.activeItemIdx >= 0) {
                    this.searchField = this.autocomplete.lists[this.activeItemIdx]
                    this.$emit('choice', this.autocomplete.lists[this.activeItemIdx])
                    this.clearList()
                }
            }
            
        },
        moveByItems(e) {
            let keyCode = e.key
            switch (keyCode) {
                case 'ArrowDown' : {
                    e.preventDefault()
                    this.activeItemIdx === false || this.activeItemIdx === this.autocomplete.lists.length - 1 //если ещё ничего не выбранно, выбираем самый первый
                        ? this.activeItemIdx = 0
                        : this.activeItemIdx++

                        this.scrollToElement()
                        
                    break;
                }
                case 'ArrowUp' : {
                    e.preventDefault()
                    this.activeItemIdx === false || this.activeItemIdx === 0 //если ещё ничего не выбранно, выбираем самый первый
                        ? this.activeItemIdx = this.autocomplete.lists.length - 1
                        : this.activeItemIdx--

                    this.scrollToElement()
                    break;
                }
            }
        },
        //Syntax
        clearList() {
            this.activeItemIdx = false;
            this.autocomplete = {
                lists: [ ]
            }
        },
        scrollToElement() {
            if (Array.isArray(this.$refs['active-items']) && this.$refs['active-items'][0]) {
                this.$refs['active-items'][0].scrollIntoView({block: "center", behavior: "smooth"})
            }
        },
        clickAnotherScope(event) { //если кликнули в другую область документа скрываем select
            let isClickAnotherScope = !event.target.closest('autocomplete')
            if (isClickAnotherScope)
                this.clearList()
        }
    },
    computed: {
        hasLists() {
            if (Array.isArray(this.autocomplete.lists) && this.autocomplete.lists.length > 0) {
                return true
            } else {
                return false
            }
        },
        activeItem() {
            return this.activeItemIdx
        }
    },  
    mounted() {
        let that = this
        this.searchField = this.searchInput
        document.addEventListener("click", that.clickAnotherScope)
    },
    destroyed() {
        let that = this
        document.removeEventListener("click", that.clickAnotherScope)
    }
}
</script>

<style scoped>
    .autocomplete {
        transition: all .2s linear;
        box-sizing: border-box;
        font-family: sans-serif;
        margin-top: 30px;
        width: 400px;
    }
    .autocomplete-input__wrap {
        width: 100%;
        position: relative;
    }
    .autocomplete-input__input {
        width: 100%;
        padding: 7px 12px;
        outline: none;
    }
    .autocomplete-list {
        list-style-type: none;
        padding: 0;
        margin: 0;
        height: 250px;
        overflow-y: auto;
        border: 1px solid red;
    }
    .autocomplete-items {
        padding: 5px 10px;
        cursor: pointer;
    }
    .autocomplete-items:hover {
        background-color: rgba(0, 0, 0, .035);
    }
    .autocomplete-items_hover{
        background-color: rgba(0, 0, 0, .075);
    }
</style>