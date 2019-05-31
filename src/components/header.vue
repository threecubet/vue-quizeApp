<template>
    <div>
        <b-nav tabs>
            <b-nav-item disabled><b>Quize app</b></b-nav-item>
            <b-nav-item disabled><b>Counter : {{numcorrect}}/{{numtotal}}</b></b-nav-item>
        </b-nav>
    </div>
</template>

<script>
export default {
    props:[
        'numcorrect',
        'numtotal'
    ]
}
</script>