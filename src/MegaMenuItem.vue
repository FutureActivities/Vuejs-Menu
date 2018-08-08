<template>
    <div class="menu__megamenu" v-if="canShow(item)" :class="[{'menu__megamenu--parent': item.links || item.custom}, itemClass]" @mouseenter="handleEnter()" @mouseleave="handleLeave()">
        <fa-menu-link class="menu__megamenu__item" :vue-router="vueRouter" :classes="item.classes" :url="item.url" v-on:click="handleClick()">
            <div v-if="item.override" v-html="item.override"></div>
            <span v-else>{{ item.name }}</span>
        </fa-menu-link>
        
        <div class="menu__megamenu__dropdown-wrapper" v-show="active" v-if="item.links || item.custom">
            
            <!-- Multilevel Menu - Displays the children on hover of the parent -->
            <div class="menu__megamenu__dropdown" v-if="display == 'multilevel'">
                <div v-if="item.pre" class="menu__pre" v-html="item.pre"></div>
                
                <ul v-if="item.links" v-for="(list,level) in item.links" class="menu__links" :class="levelClass(level)">
                    <li v-for="link in list" :class="levelClass(level)" v-on:mouseover="menuItemHover(link.id, level)">
                        <fa-menu-link :vue-router="vueRouter" :url="link.url" :classes="[{'active': isActive(link.id, level)}, link.classes]" v-on:click="handleClick()">
                            <div v-if="link.prefix" class="prefix" v-html="link.prefix"></div>
                            <div v-if="link.img" class="image"><img :src="link.img" :alt="link.name" /></div>
                            <span class="name">{{ link.name }}</span>
                            <div v-if="link.suffix" class="suffix" v-html="link.suffix"></div>
                        </fa-menu-link>
                    </li>
                </ul>
                
                <div v-if="item.custom" class="menu__custom" v-html="item.custom"></div>
            </div>
            
            <!-- Columns Menu - Supports 1 level only and displays underneath the parent link in columns -->
            <div class="menu__megamenu__columns" v-if="display == 'columns'">
                <div v-if="item.pre" class="menu__pre" v-html="item.pre"></div>
                
                <div v-if="item.links" v-for="(list,level) in item.links" class="menu__links">
                    <div v-for="link in list" :class="levelClass(level)" class="menu__links__parent">
                        <div v-if="link.prefix" class="prefix" v-html="link.prefix"></div>
                        <fa-menu-link :vue-router="vueRouter" :url="link.url" class="menu__links__heading" :classes="item.classes" v-if="link.url" v-on:click="handleClick()">{{ link.name }}</fa-menu-link>
                        <span class="menu__links__heading" :class="item.classes" v-else>{{ link.name }}</span>
                        <ul>
                            <li v-for="child in getChildren(link.id)">
                                <fa-menu-link :vue-router="vueRouter" :url="child.url" :classes="child.classes" v-on:click="handleClick()">
                                    <div v-if="child.prefix" class="prefix" v-html="child.prefix"></div>
                                    <div v-if="child.img" class="image"><img :src="child.img" :alt="child.name" /></div>
                                    <span class="name">{{ child.name }}</span>
                                    <div v-if="child.suffix" class="suffix" v-html="child.suffix"></div>
                                </fa-menu-link>
                            </li>
                        </ul>
                        <div v-if="link.suffix" class="suffix" v-html="link.suffix"></div>
                    </div>
                </div>
                
                <div v-if="item.custom" class="menu__custom" v-html="item.custom"></div>
            </div>
        </div>
    </div>
</template>

<script>
import FaLink from './Link.vue'

export default {
    name: 'fa-menu-mega',
    data: function() {
        return {
            active: false,
            activeWaiting: false
        };
    },
    props: {
        item: {
            type: Object,
            required: true
        },
        itemClass: {
            type: String,
            default: ''
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
    methods: {
        /**
         * Generate the ul class
         */
        levelClass: function(level) {
            return 'level'+level;
        },
        /**
         * Check if a menu item is visible
         */
        canShow: function(item) {
            if (typeof item.bar === 'undefined')
                return true;
                
            return item.bar;
        },
        /**
         * Check if a category is active to apply
         * the active class
         */
        isActive: function(id, level) {
            if (typeof this.item.active == 'undefined')
                this.item.active = [];
                
            var active = this.item.active[level];
            if (this.item.source && this.item.source[id]) {
                var children = this.item.source[id];
                return children && active === id;
            }
        },
        getChildren: function(id) {
            if (this.item.source && this.item.source[id])
                return this.item.source[id];
                
            return null;
        },
        /**
         * Desktop Megamenu
         * On hover show the child menu if available.
         */
        menuItemHover: function(id, level) {
            
            // Set this category as the active one for the level
            this.item['active'][level] = id;
            
            // Determine the next level from this point
            var currLevel = parseInt(level);
            var nextLevel = currLevel + 1;
            
            // Remove everything in the array after the next level
            // Ensures deeper categories are removed when hovering over a higher level
            this.item.links.length = nextLevel;
            
            // Set the children
            var children = this.getChildren(id);
            if (children)
                this.item.links.push(children);
            
            this.$forceUpdate();
        },
        /**
         * Navigation
         * Handles the click of a navigation item
         */
        handleClick: function(){
           this.$emit('click'); 
           this.active = false;
        },
        handleEnter: function(){
            this.activeWaiting = true;
            setTimeout(() => {
                if (this.activeWaiting)
                    this.active = true;
                this.activeWaiting = false;
            }, this.hoverDelay);
        },
        handleLeave: function(){
            this.active = false;
            this.activeWaiting = false;
        }
    },
    components: {
        'fa-menu-link': FaLink
    }
}
</script>