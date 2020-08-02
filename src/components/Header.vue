<template>
    <div>
        <b-nav tabs>
            <b-nav-item disbled><b>My Quiz App</b></b-nav-item>
            <b-nav-item disabled> Counter: {{numCorrect}}/{{numTotal}} </b-nav-item>
        </b-nav>
    </div>
</template>


<script>
export default {
    props:[ // received numCorrect from app.vue
        'numCorrect',
        'numTotal'
    ]
}
</script>