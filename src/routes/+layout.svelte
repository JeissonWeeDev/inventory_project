<script>
  import Header from "../components/Header.svelte";
  import Footer from "../lib/Footer.svelte";
</script>

<main class="cnt_main">
  <Header />
  <main class="cnt_content_pages">
    <slot />
  </main>
  <Footer />
</main>

<!-- /* onMount(() => {
  // Ingreso de datos de prueba
  const objtest = {
    products: [
      { name: "vestido", precio: 60 },
      { name: "exclavo", precio: 30 },
    ],
  };
  localStorage.setItem("username", "Jeisson");
  localStorage.setItem("objects", JSON.stringify(objtest));

  // Recupera la información del usuario de localStorage
  const storedUsername = localStorage.getItem("username");
  const storedObjects = localStorage.getItem("objects");

  if (storedUsername) {
    user.username = storedUsername;
    if (storedObjects) {
      user.objects = JSON.parse(storedObjects);
    }

    console.log("Usuario autenticado:", user);
  } else {
    console.log("Usuario no autenticado");
    // Redirige al inicio de sesión
    /* redirect('/login');
  } 
}); */ -->

<style>
  /* Principal container */
  .cnt_main {
    position: fixed;
    display: flex;
    flex-direction: column;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
    width: 100%;
    background-color: var(--background-color);
    overflow-y: auto;
    overflow-x: hidden;
  }

  /* Container content in pages */
  .cnt_content_pages {
    display: flex;
    flex-direction: column;
    flex-grow: 1;
    height: 0;
    min-height: auto;
  }
</style>
