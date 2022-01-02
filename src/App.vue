<template>
  <!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Калькулятор</title>
</head>
  <body>
      <calculation />
  </body>
</html>
</template>

<script>
import calculation from './components/calculation.vue'

export default {
  name: 'App',
  components: {
    calculation
  }
}
</script>
