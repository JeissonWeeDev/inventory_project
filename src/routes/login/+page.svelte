<script>
  import Modal from "../../components/Modal.svelte";
  import Slider from "../../components/Slider.svelte";
  import { onMount } from "svelte";

  // Recursos ---------
  import backgroundPic from "$lib/assets/backLoginRegis.jpg";

  let showModalState = false;
  let modalContent = {};

  function showModal(content) {
    showModalState = true;
    modalContent = content;
  }

  onMount(() => {
    // Llama a la función showModal con el objeto JSON como parámetro
    showModal({
      modalConfig: {
        userClose: false,
        maxWidth: "600px",
      },
      modalCont: {
        component: Slider,
      },
    });
  })
</script>

<section>
  <!-- Componente Modal y sistema de despliegue modal -->
  {#if showModalState}
    <Modal bind:showModalState {modalContent} />
  {/if}

  <img class="background-pic" src={backgroundPic} alt="Background Cover" />
</section>

<style>
  .background-pic {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    width: 100%;
    height: 100dvh;
    object-fit: cover;
    margin: auto;
    z-index: 1;
    animation: backani 20s linear infinite;
  }
  @keyframes backani {
    from {
      scale: 3;
      transform: rotate(0deg);
    }
    to {
      scale: 3;
      transform: rotate(360deg);
    }
  }
</style>
