<template>
   <div>
      <b-navbar tabs>
       
      <b-navbar-nav>
       <b-nav-item disabled>Fancy Quiz App</b-nav-item>
        <b-nav-item disabled>Counter {{numCorrect}}/{{numTotal}}</b-nav-item>
      </b-navbar-nav>
      </b-navbar>
</div>
</template>

<script>
export default {
   props: [
      'numCorrect',
      'numTotal'
   ]

}
</script>