<script setup lang="ts">
import HtmlListUser from './components/HtmlListUser.vue';
import PugListUser from './components/PugListUser.vue';

</script>

<template>
    <HtmlListUser></HtmlListUser>

    <!-- NOTE: removing the below component and building, the app works perfectly -->
    <!-- as soon as we include it like so, we get zero typecheck errors, and many runtime errors -->
    <!-- it seems that using a Pug template will bypass the type checker entirely within templates -->
    <PugListUser></PugListUser>
</template>