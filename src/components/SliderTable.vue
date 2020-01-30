<template>
  <div class="vue-slider-table">
    <b-table sticky-header :items="items" head-variant="light"></b-table>
  </div>
</template>

<script>
  export default {
    name: "SliderTable",
    data() {
      return {
        items: [
          { heading1: 'table cell', heading2: 'table cell', heading3: 'table cell' },
          { heading1: 'table cell', heading2: 'table cell', heading3: 'table cell' },
          { heading1: 'table cell', heading2: 'table cell', heading3: 'table cell' },
          { heading1: 'table cell', heading2: 'table cell', heading3: 'table cell' },
          { heading1: 'table cell', heading2: 'table cell', heading3: 'table cell' },
          { heading1: 'table cell', heading2: 'table cell', heading3: 'table cell' },
          { heading1: 'table cell', heading2: 'table cell', heading3: 'table cell' },
          { heading1: 'table cell', heading2: 'table cell', heading3: 'table cell' },
          { heading1: 'table cell', heading2: 'table cell', heading3: 'table cell' },
          { heading1: 'table cell', heading2: 'table cell', heading3: 'table cell' },
          { heading1: 'table cell', heading2: 'table cell', heading3: 'table cell' },
          { heading1: 'table cell', heading2: 'table cell', heading3: 'table cell' }
        ]
      }
    }
  }
</script>