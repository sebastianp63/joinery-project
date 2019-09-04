<template>
  <div class="uk-card uk-card-default uk-card-body">
    <div class="uk-tile uk-tile-secondary uk-padding-small uk-margin-small-bottom">
      <p class="uk-h4">Your templates</p>
    </div>
    <table class="uk-table">
      <thead>
        <tr>
          <th>Table Heading</th>
          <th>Table Heading</th>
          <th>Table Heading</th>
        </tr>
      </thead>
      <tfoot>
        <tr>
          <td>Table Footer</td>
          <td>Table Footer</td>
          <td>Table Footer</td>
        </tr>
      </tfoot>
      <tbody>
        <tr>
          <td>Table Data</td>
          <td>Table Data</td>
          <td>Table Data</td>
        </tr>
        <tr>
          <td>Table Data</td>
          <td>Table Data</td>
          <td>Table Data</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: "templateContainer"
};
</script>


<style lang="scss" scoped>
.uk-card {
  min-height: 100%;
}
</style>