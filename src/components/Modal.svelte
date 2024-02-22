<!-- Componente plantilla de modal para el uso general -->

<script>
  export let showModalState;
  export let modalContent;

  // Configuracion para el modal
  let { userClose } = modalContent.modalConfig;

  // Componente de entrada
  let { component } = modalContent.modalCont;

  // Metodo para el cierre del modal
  function closeModal() {
    showModalState = false;
  }
</script>

<!-- Modal card -->
<div class="modal-body">
  <header class="modal-header-cont">
    <span class="lg-icon">
      <svg
        width="40"
        height="40"
        viewBox="0 0 40 40"
        fill="none"
        xmlns="http://www.w3.org/2000/svg"
        ><rect
          x=".5"
          y=".5"
          width="39"
          height="39"
          rx="19.5"
          stroke="#999"
        /><mask
          id="a"
          style="mask-type:luminance"
          maskUnits="userSpaceOnUse"
          x="10"
          y="10"
          width="20"
          height="20"><path d="M30 10H10v20h20V10Z" fill="#fff" /></mask
        ><g mask="url(#a)"
          ><mask
            id="b"
            style="mask-type:alpha"
            maskUnits="userSpaceOnUse"
            x="4"
            y="14"
            width="19"
            height="18"
            ><path
              d="M16.653 31.015 4.926 22.892l6.13-8.85 11.727 8.123-6.13 8.85Z"
              fill="#fff"
            /></mask
          ><g mask="url(#b)"
            ><mask
              id="c"
              style="mask-type:alpha"
              maskUnits="userSpaceOnUse"
              x="4"
              y="14"
              width="19"
              height="18"
              ><path
                d="M16.653 31.015 4.926 22.892l6.13-8.85 11.727 8.123-6.13 8.85Z"
                fill="#fff"
              /></mask
            ><g mask="url(#c)"
              ><mask
                id="d"
                style="mask-type:alpha"
                maskUnits="userSpaceOnUse"
                x="10"
                y="14"
                width="13"
                height="14"
                ><path
                  d="m12.081 14.752 9.677 6.703a1.248 1.248 0 0 1-.278 2.196l-9.442 3.453c-.431.151-.9.09-1.27-.165a1.381 1.381 0 0 1-.594-1.126l-.05-10.029c-.002-.468.254-.89.662-1.11a1.256 1.256 0 0 1 1.295.078Z"
                  fill="#fff"
                /></mask
              ><g mask="url(#d)"
                ><path
                  d="m18.38 32.975 9.952-14.367-14.248-9.869-9.952 14.367 14.248 9.87Z"
                  fill="#999"
                /></g
              ></g
            ></g
          ><mask
            id="e"
            style="mask-type:alpha"
            maskUnits="userSpaceOnUse"
            x="17"
            y="8"
            width="19"
            height="18"
            ><path
              d="m23.346 8.985 11.728 8.123-6.13 8.85-11.728-8.124 6.13-8.85Z"
              fill="#fff"
            /></mask
          ><g mask="url(#e)"
            ><mask
              id="f"
              style="mask-type:alpha"
              maskUnits="userSpaceOnUse"
              x="17"
              y="8"
              width="19"
              height="18"
              ><path
                d="m23.346 8.985 11.728 8.123-6.13 8.85-11.728-8.124 6.13-8.85Z"
                fill="#fff"
              /></mask
            ><g mask="url(#f)"
              ><mask
                id="g"
                style="mask-type:alpha"
                maskUnits="userSpaceOnUse"
                x="17"
                y="12"
                width="13"
                height="14"
                ><path
                  d="m27.919 25.248-9.677-6.703a1.248 1.248 0 0 1 .278-2.196l9.442-3.453c.43-.151.9-.09 1.27.165.368.256.59.674.594 1.126l.05 10.028c.002.469-.255.89-.662 1.11a1.256 1.256 0 0 1-1.295-.077Zm.652-11.234a.222.222 0 0 0-.202-.026l-9.442 3.454c-.01.001-.049.019-.06.073a.08.08 0 0 0 .037.087l9.677 6.703c.012.008.041.029.094.004.053-.026.045-.066.048-.082L28.66 14.19a.211.211 0 0 0-.089-.176Z"
                  fill="#fff"
                /></mask
              ><g mask="url(#g)"
                ><path
                  d="m21.62 7.024-9.952 14.368 14.248 9.869 9.952-14.367-14.248-9.87Z"
                  fill="#999"
                /></g
              ></g
            ></g
          ></g
        ></svg
      >
    </span>
    <button
      class="btn-close"
      style="display: {userClose ? 'flex' : 'none'};"
      on:click={closeModal}
    >
      <svg
        width="16"
        height="16"
        viewBox="0 0 16 16"
        fill="none"
        xmlns="http://www.w3.org/2000/svg"
        ><path
          fill-rule="evenodd"
          clip-rule="evenodd"
          d="M.372 13.833a1.27 1.27 0 0 0 1.795 1.795L8 9.795l5.833 5.833a1.27 1.27 0 0 0 1.795-1.795L9.795 8l5.833-5.833A1.27 1.27 0 1 0 13.833.372L8 6.205 2.167.372A1.27 1.27 0 0 0 .372 2.167L6.205 8 .372 13.833Z"
          fill="#8397AF"
        />
      </svg>
    </button>
  </header>

  <!-- Cuerpo del contenido del modal -->
  <!-- {modalContent.message} -->
  <main class="modal-main-cont">
    <svelte:component this={component} />
  </main>
</div>

<!-- Capa de no visualizacion de contenido -->
<!-- svelte-ignore a11y-click-events-have-key-events -->
<!-- svelte-ignore a11y-no-static-element-interactions -->
<div
  class="modal-backg-cont"
  style="display: {showModalState ? 'block' : 'none'};"
  on:click={userClose ? closeModal : null}
/>

<style>
  .modal-backg-cont {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    width: 100%;
    height: 100svh;
    overflow: hidden;
    background-color: rgba(0, 0, 0, 0.8);
    z-index: 10;
  }

  .modal-body {
    position: absolute;
    left: 0;
    right: 0;
    bottom: 0;
    margin: auto;
    width: 100%;
    max-width: 45rem;
    height: max-content;
    min-height: 4.5rem;
    background-color: hsl(212, 24%, 8%);
    border-radius: 1.5rem 1.5rem 0 0;
    padding: 2rem 0 0 0;
    overflow: hidden;
    z-index: 11;
  }

  /* Header de modal */
  .modal-header-cont {
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    display: flex;
    justify-content: space-between;
    align-items: flex-end;
    padding: 1rem 1rem 0 1rem;
    width: inherit;
    height: max-content;
    z-index: 12;
  }

  .lg-icon {
    width: 2.5rem;
    height: 2.5rem;
  }
  .lg-icon svg {
    width: inherit;
    height: inherit;
  }

  .btn-close {
    cursor: pointer;
    display: flex;
    justify-content: center;
    align-items: center;
    width: 2.5rem;
    height: 2.5rem;
    background-color: #23262B;
    border-radius: 20px;
    --cl-hover: #8397af;
  }
  .btn-close:hover {
    --cl-hover: #c1cbd7;
  }
  .btn-close svg {
    height: 1rem;
    width: 1rem;
  }
  .btn-close svg path {
    fill: var(--cl-hover);
  }
</style>
