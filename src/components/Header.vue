<template>
<div>
    <b-nav tabs>
    <b-nav-item disabled><b>Quiz Time App</b></b-nav-item>
    <b-nav-item disabled>Counter: {{countCorrect}}/{{countTotal}}</b-nav-item>
    </b-nav>
</div>
</template>

<script>
export default {
    props: [
        'countTotal',
        'countCorrect'
    ]
}
</script>