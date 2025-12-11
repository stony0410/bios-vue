<template>
  <div>
    <h2>System Time</h2>
    <p>{{ time }}</p>

    <h2>System Date</h2>
    <p>{{ date }}</p>

    <br />
    <p>▶ SATA Devices</p>
    <p>(Press Enter to open)</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      time: "16:30:35",
      date: "Thu 12/27/2012"
    };
  }
};
</script>
