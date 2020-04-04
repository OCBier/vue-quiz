<template>
     <b-nav>
        <b-nav-item disabled><b>Amazing Quiz App</b></b-nav-item>
        <b-nav-item disabled>Score: {{numCorrect}} / {{numTotal}}</b-nav-item>
        
  </b-nav>
</template>

<script>
export default {
    props: {
        numCorrect: {
            default: 0,
            type: Number
        },        
        numTotal: {
            default: 0,
            type: Number
        }
    }

}
</script>