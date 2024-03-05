<script>
  export let width;
  export let height;
</script>

<svg
  class="ready-icon"
  {width}
  {height}
  viewBox="0 0 40 40"
  fill="none"
  xmlns="http://www.w3.org/2000/svg"
>
  <path
    fill-rule="evenodd"
    clip-rule="evenodd"
    d="M0 19.383v.004c0 .593.224 1.186.67 1.643l11.766 12.032a2.353 2.353 0 0 0 3.365 0L39.329 8.998a2.353 2.353 0 1 0-3.364-3.29L14.118 28.052 4.035 17.74A2.353 2.353 0 0 0 0 19.383"
    fill="var(--color-main)"
  />
</svg>

<style>
  .ready-icon {
    animation: readyani 0.3s ease;
  }
  @keyframes readyani {
    from {
      opacity: 0;
      transform: translateY(8px);
    }
    to {
      opacity: 1;
      transform: translateY(0px);
    }
  }
</style>
