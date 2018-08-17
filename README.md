# Future Activities Menu

Provides components to build a mega menu for desktop views and for mobile a multi-page navigation view that loads the child links on each click (includes back buttons).

## Demo

Coming Soon

## Getting Started

Import the MegaMenu and BurgerMenu components:

    import { MegaMenu, BurgerMenu } from 'fa-vue-menu';
    
### Mega Menu

The mega menu component will output a menu with each item a div containing the child links that will appear on hover of the parent.

    <fa-menu-mega :data="data" display="multilevel"></fa-menu-mega>
    
When displaying the menu you can also set how the menu should display, currently this component provides 2 options: `columns` and `multilevel` with multilevel being the default.
The columns view will display each link as a heading of a column and then render 1 level of children underneath it.
The multilevel view will list all the links in column 1 and then as you hover over a link the children are displayed in the next column.
    
### Burger Menu

The burger menu component will output the top level of links and when clicked they will be replaced with the next level, etc.
This creates a multi-page menu ideal for mobile burger menus.

    <fa-menu-burger :data="data"></fa-menu-burger>
    
### Data Structure

Structure your data object in the following format:

    {
       "category": {
          "name":"Category",
          "source": {
              "1": [
                {
                  "id": 3,
                  "name": "Child Link",
                  "url": "#"
                },
                {
                  "id": 4,
                  "name": "Another Child Link",
                  "url": "#"
                }
              ],
              "4": [
                {
                  "id": 5,
                  "name": "Next level child",
                  "url": "#"
                }
              ]
          },
          "links":[
             [
                {
                  "id":1,
                  "name":"Parent",
                  "url":"#"
                },
                {
                  "id":2,
                  "name":"Other",
                  "url":"#",
                  "img":"url",
                  "prefix":"<strong>custom html</strong>",
                  "suffix":"<strong>custom html</strong>"
                }
             ]
          ],
          "classes": ["custom-class"],
          "pre": "<p>Custom HTML that will be displayed on the full Mega Menu before the links.</p>",
          "custom": "<p>Custom HTML that will be displayed on the full Mega Menu after the links.</p>"
       },
       "standalone":{
          "name":"Standalone Link",
          "url":"#",
          "bar": true,
          "burger" true
       },
       "mylink": {
           "name":"Another Link",
           "override":"<strong>LINK</strong>"
       }
    }
    
`links` is the data that is initially displayed in the megamenu dropdown. This is an array where each item will create a new UL element in the mega menu.

`source` is for the child menu data. This should be organised by the parent ID. When you hover or click on a menu item it will attempt to see if that item has any children by checking for the ID in source.
Each item can also be hidden from the horizontal mega menu or the burger menu, so you can include links that are only visible on the burger menu, for example.

`override` allows you to use your own HTML in the horizontal bar menu item.

`custom` allows you to use your own HTML inside the horizontal bar mega menu.

#### Options

| Prop | Description | Type | Default | Component |
|--|--|--|--|--|
| data | The menu data object. Use this or feed. | Object | null | MegaMenu,BurgerMenu |
| vue-router | Use `router-link` instead of `a` tags | Boolean | false | MegaMenu,BurgerMenu |
| display | Display the menu as `columns` or `multilevel` | String | multilevel | MegaMenu |
| hover-delay | Delay the opening of the menu on hover. Time in milliseconds. | Number | 0 | MegaMenu |
