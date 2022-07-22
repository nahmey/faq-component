# Faq-component

<!-- ![alt text](https://julien-kennel.fr/images/git/table.PNG) -->

Simple FAQ for VUE.JS made with Bootstrap

- [Dependencies](#dependencies)
- [Installation](#installation)
- [Database structure](#database)
- [Usage](#example-usage)
- [Props](#props)
- [Example](#example)

## Dependencies
* Vue.js. Required.
* Bootstrap (CSS). Required.

## Installation

Install with npm:
```bash
npm i faq-component
```

## Database
table categories :
- id (integer PK)
- nom (varchar 255)
- nom_interne (varchar 255)

table faqs :
- id (ineger PK)
- categorie_id (integer FK categories)
- object (varcher 255)
- question (text)
- reponse (text)

Import globally in app.js:

```javascript
import FaqComponent from 'faq-component';
Vue.component('faq-component', FaqComponent);
```

## Usage
```html
<faq-component :api_url="'your_api_url'"></faq-component>
```

## Props

| Name&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | Type | Description | Default
| ----------------- | :--- | :--- | :--- |
| `api_url`      | `String` | URL that calls data |  |

## Example
```html
<faq :api_url="'http://your/api/url'"></faq>
```