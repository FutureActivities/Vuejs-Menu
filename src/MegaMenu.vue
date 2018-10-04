<template>
    <div class="menu">
        <fa-menu-megaitem v-for="(item,key) in data" 
            :key="key" 
            :item="item" 
            :item-class="key" 
            :components="components" 
            :display="display" 
            :vue-router="vueRouter" 
            :hover-delay="hoverDelay" 
            v-on:click="$emit('click')" 
            v-on:enter="$emit('enter', item)" 
            v-on:leave="$emit('leave', item)"
            v-on:linkEnter="handleLinkEnter"
            v-on:linkLeave="handleLinkLeave">
        </fa-menu-megaitem>
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
        },
        components: {
            type: Object,
            default() {
                return {};
            },
            required: false
        }
    },
    methods: {
        handleLinkEnter: function(link) {
            this.$emit('linkEnter', link);
        },
        handleLinkLeave: function(link) {
            this.$emit('linkLeave', link);
        }
    },
    components: {
        'fa-menu-megaitem': MenaMenuItem
    }
}
</script>