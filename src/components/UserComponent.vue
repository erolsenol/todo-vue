<template>
    
    <h4 v-html="'Title: \'' + username.title + '\' Order: \'' + username.order + '\''"></h4>
    
        
</template>

<script>
export default {
    name: 'UserComponent',
    props: {
        username: Object
    },
    data: function () {
        return {
            
        }
    }
}
</script>
