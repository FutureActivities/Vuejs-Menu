<template>
    <div class="menu">
        <fa-menu-megaitem v-for="(item,key) in data" :key="key" :item="item" :item-class="key" :display="display" :vue-router="vueRouter" :hover-delay="hoverDelay"></fa-menu-megaitem>
    </div>
</template>

<script>
import MenaMenuItem from './MegaMenuItem.vue'

export default {
    name: 'fa-menu-mega',
    data: function() {
        return {
        };
    },
    props: {
        data: {
            type: Object,
            required: true
        },
        display: {
            type: String,
            default: 'multilevel'
        },
        vueRouter: {
            type: Boolean,
            default: false,
            required: false
        },
        hoverDelay: {
            type: Number,
            default: 0,
            required: false
        }
    },
    components: {
        'fa-menu-megaitem': MenaMenuItem
    }
}
</script>