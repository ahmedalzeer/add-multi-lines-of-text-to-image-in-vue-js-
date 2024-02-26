# add-multi-text-to-img

A Vue component for adding multi-lines of text to an image.

#### Installation

```bash
npm install add-multi-text-to-img
```
#### Usage

vue

npm install add-multi-text-to-img

Usage

vue

``` javascript
<template>
    
    <AddMultiTextToImg @textLinesSaved="handleTextLinesSaved"></AddMultiTextToImg>
</template>

<script>
import AddMultiTextToImg from 'add-multi-text-to-img';

export default {
    components: {
        AddMultiTextToImg,
    },
    methods: {
        handleTextLinesSaved(textLines) {
            console.log('Received text lines and background image :', data);
        },
    }
};
</script>
