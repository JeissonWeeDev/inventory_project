<script>
  export let width;
  export let height;
  export let timeAnimation;
</script>

<svg
  class="loading-icon"
  {width}
  {height}
  viewBox="0 0 40 40"
  style={`--animation-duration: ${timeAnimation};`}
  fill="none"
  xmlns="http://www.w3.org/2000/svg"
>
  <path
    d="M0 20A20 20 0 1 0 34.362 6.081l-4.309 4.176A14 14 0 1 1 6 20z"
    fill="var(--color-contrast-160)"
  />
  <path
    d="M40 20A20 20 0 1 0 5.638 33.919l4.308-4.176A14 14 0 1 1 34 20z"
    fill="var(--color-contrast-500)"
  />
  <circle
    cx="7.654"
    cy="31.689"
    r="3"
    transform="rotate(45.892 7.654 31.689)"
    fill="var(--color-contrast-500)"
  />
  <circle cx="37" cy="19.8" fill="var(--color-contrast-500)" r="3" />
</svg>

<style>
  .loading-icon {
    animation: spinloading var(--animation-duration, 1s) linear infinite;
  }
  @keyframes spinloading {
    0% {
      transform: rotate(0deg);
    }
    100% {
      transform: rotate(360deg);
    }
  }
</style>
