<template>
  <div>
      {{item.subDistrict}} » {{item.district}} » {{item.province}} » {{item.Zipcode}}
  </div>
</template>

<script>
export default {
  props: {
    item: { required: true },
  },
};
</script>

<style>

</style>