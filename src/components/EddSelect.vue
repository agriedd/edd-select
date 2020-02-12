<template>
    <div class="form-group position-relative">
        <label for="select" class="small text-muted">Select</label>
        <div class="form-control form-control-sm _reference shadow-none" v-on:click="togglepopper()" style="height: auto">
            <div v-if="selected != null">
                <div class="font-weight-bold">
                    {{ getselectedItem().item.label }}
                </div>
                <div class="small text-muted">
                    {{ getselectedItem().item.description }}
                </div>
            </div>
            <div v-else>
                Select...
            </div>
        </div>
        <div class="dropdown-menu shadow border-0 w-100 _popper show py-3" v-if="show">
            <div class="dropdown-item border-0 py-3" v-for="(item) in items" v-bind:key="item.id"
                :id="item.html_id"
                :class="[ isactive(item.id) ? `__active` : null, {  }]"
                v-on:click="selectitem(item)">
                <div class="d-flex justify-content-between">
                    <div>
                        <div class="small font-weight-bold">
                            {{ item.label }}
                        </div>
                        <div class="small">
                            {{ item.description }}
                        </div>
                    </div>
                    <div v-if="isactive(item.id)">
                        ✅
                    </div>
                </div>
            </div>
            <div class="dropdown-divider"></div>
            <div class="d-flex">
                <button class="btn btn-light btn-sm shadow-none bg-white px-3 ml-auto border-0" v-on:click="closepopper()">
                    Done
                </button>
            </div>
        </div>
    </div>
</template>
<script>
const {createPopper} = require('@popperjs/core')

export default {
    props: {
        options: { type: Array, required: true },
    },
    name: "eddselect",
    data(){
        return {
            id: "",
            data: {
                selected: null,
                show: false,
            },
            popper: null
        }
    },
    computed: {
        show: {
            get(v){
                return this.data.show
            },
            set(v){
                if(v) this.$nextTick(()=> this.setpopper()); else this.removepopper();
                this.data.show = v
            }
        },
        offsety(){
            return this.getoffsettopitem(this.getselectedItem())
        },
        selected: {
            get(){
                return this.data.selected
            },
            set(v){
                if(this.popper) this.$nextTick(()=> this.popper.update())
                console.log(this.popper);
                this.data.selected = v
            }
        },
        items(){
            return this.options.filter(e => true).map(e => ({
                id: e.id,
                label: e.label,
                description: `${e.label}`,
                html_id: `__${this.id}-${e.id}`
            }))
        }
    },
    methods: {
        togglepopper(){
            this.show = !this.show;
        },
        closepopper(){
            this.show = false;
        },
        selectitem(item){
            this.selected = item.id
            this.$nextTick(()=>{
                this.closepopper()
            })
        },
        getitembyid(id){
            let item = this.items.find(i => String(i.id).toLowerCase() == String(id).toLowerCase() );
            return item ? Object.assign({},item) : null;
        },
        getpositionbyitem(item){
            let position = this.items.findIndex(i => String(i.id).toLowerCase() == String(item.id).toLowerCase())
            return position
        },
        getselectedItem(){
            let item = this.getitembyid(this.selected) //this.selected
            if(item) return {
                el: this.show ? this.$el.querySelector(`#${item.html_id}`) : null,
                item: item,
                position: this.getpositionbyitem(item)
            }
            return null;
        },
        getoffsettopel(el, parent){
            if(el){
                var offset = el.offsetTop
                offset += el.offsetHeight
                while(el = el.offsetParent){
                    if(el == parent)
                        break;
                    offset += el.offsetTop
                }
                return offset
            }
            return 0
        },
        getoffsettopitem(item){
            if(item)
                return this.getoffsettopel(item.el, this.$el.querySelector("._popper"));
            return 0
        },
        isactive(id){
            return this.selected == id
        },
        removepopper(){
            this.popper.destroy;
            this.popper = null;
        },
        setpopper(){
            this.$nextTick(()=>{
                var referencepopper = this.$el.querySelector("._reference")
                let targetpopper = this.$el.querySelector("._popper")
                // window.el = targetpopper
                this.popper = createPopper(referencepopper, targetpopper, {
                    modifiers: [
                        {
                            name: "offset",
                            options: {
                                offset: (placement)=>{
                                    return [0, this.offsety * -1];
                                }
                            }
                        }
                    ]
                })
            })
        }
    },
    created(){
        this.id = `id${Date.now()}`
    },
    mounted(){
        // window.el = y_offset
        
    }
}
</script>
<style lang="scss" scoped>
    $success: #f28;
    .__active{
        border-left: 3px solid $success !important;
        color: darken($success, 20%);
    }
</style>