<template>
    <div class="menu__mobile-menu">
        <slot name="prefix"></slot>
        <ul v-if="current.length == 0">
            <li v-for="(category,key) in data" v-if="canShow(category)"  class="menu__mobile-menu__item menu__mobile-menu__item--top" :class="[key]">
                <div v-if="category.prefix" class="prefix" v-html="category.prefix"></div>
                <fa-menu-link :url="category.url" v-on:click.stop.prevent="setType(key, category)">{{ category.name }}</fa-menu-link>
                <div v-if="category.suffix" class="suffix" v-html="category.suffix"></div>
            </li>
        </ul>
        
        <ul v-if="current.length > 0">
            <li class="menu__mobile-menu__item menu__mobile-menu__item--back" v-on:click="back">Back</li>
            <li class="menu__mobile-menu__item menu__mobile-menu__item--parent">
                <div v-if="previous().prefix" class="prefix" v-html="previous().prefix"></div>
                <fa-menu-link class="link" v-if="previous().url !== null" :vue-router="vueRouter" :url="previous().url" v-on:click="$emit('click')">{{ previous().name }}</fa-menu-link>
                <span  class="link" v-else>{{ previous().name }}</span>
                <div v-if="previous().suffix" class="suffix" v-html="previous().suffix"></div>
            </li>
            <li v-for="category in current" class="menu__mobile-menu__item" :class="[{'menu__mobile-menu__item--children': hasChildren(category.id)}]">
                <div v-if="category.prefix" class="prefix" v-html="category.prefix"></div>
                <fa-menu-link  class="link" :url="category.url" v-on:click.stop.prevent="item(category)">{{ category.name }}</fa-menu-link>
                <div v-if="category.suffix" class="suffix" v-html="category.suffix"></div>
            </li>
        </ul>
        <slot name="suffix"></slot>
    </div>
</template>

<script>
import FaLink from './Link.vue'

export default {
    name: 'fa-menu-burger',
    data: function() {
        return {
            type: null,
            history: [],
            current: []
        };
    },
    props: {
        data: {
            type: Object,
            required: true
        },
        vueRouter: {
            type: Boolean,
            default: false,
            required: false
        }
    },
    methods: {
        hasChildren: function(categoryId) {
            if (this.data[this.type].source)
                return this.data[this.type].source[categoryId];
                
            return false;
        },
        
        /**
         * Check if a menu item is visible
         */
        canShow: function(item) {
            if (typeof item.burger === 'undefined')
                return true;
                
            return item.burger;
        },

        /**
         * Show the appropriate submenu
         */
        setType: function(key, category) {
            if (typeof category.onclick !== 'undefined') // Optionally override default behaviour
                return window[category.onclick](category);
            
            if (!('links' in this.data[key]) && category.url)
                return this.goTo(category.url);

            this.type = key;
            this.addHistory({
                'name': this.data[key].name,
                'url': null
            });
            
            var merged = [].concat.apply([], this.data[key].links);
            
            this.addHistory(category);
            this.current = merged;
        },

        /**
         * Show the child menu if available.
         */
        item: function(category) {
            var children = this.hasChildren(category.id);
            if (typeof children === 'undefined' || !children) {
                this.goTo(category.url);
            } else {
                this.addHistory(category);
                this.current = children;
            }
        },

        /**
         * Pressing back in the burger menu
         */
        back: function() {
            var previous = this.history.pop();
            this.current = previous ? previous.links : [];
        },

        /**
         * Add the current category to the menu history
         * before showing the next level
         */
        addHistory: function(category) {
            this.history.push({
                'category': category,
                'links': this.current
            });
        },

        /**
         * Go back to the previous menu
         */
        previous: function() {
            var previous = this.history[this.history.length - 1];
            return previous.category;
        },
        
        /**
         * Go to a URL
         */
        goTo: function(url) {
            this.$emit('click');
            
            if (this.vueRouter)
                this.$router.push(url);
            else
                window.location.href = url;
        }
    },
    components: {
        'fa-menu-link': FaLink
    }
}
</script>